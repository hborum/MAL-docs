Demo.fmal
====================================


.. code-block:: text
  :linenos:
  
    data BaseEntity
      Input  : Input
      Result : Result
      Param  : Param
      Name   : String
    end
    data Input
    end
    data Result
    end
    data Param
    end

    data Projection
    	PeriodStartTime  : Float
    	PeriodEndTime 	 : Float
    	PeriodLength 	 : Float
    end

    /* Asset */
    data Asset extends BaseEntity
    	Input  : AssetInput
      	Weight : Float
    end

    data AssetInput extends Input
      	Weight : Float
    end

    /* Equity */
    data Equity extends BaseEntity
    	Input  	 : EquityInput
    	Result 	 : EquityResult
    	Param  	 : EquityParam
    	Assets   : List<Asset>
    	Policies : List<Policy>

    	Reserve 							: Float
    	PremiumScale 			  			: Float
    	BenefitScale 			  			: Float
    	InvestmentReturnTaxPaymentForPeriod : Float
    	InvestmentReturnTaxAsset 			: Float

    	PremiumPayingCashFlowNames 	  : List<string>
    	BenefitReceivingCashFlowNames : List<string>
    end

    data EquityInput extends Input
      	Reserve : Float
    end

    data EquityResult extends Result
    	PeriodBenefits 			: Float
    	PeriodPremiums 			: Float
    	AccumulatedReserveAtEnd : Float
    	IsCalculationEndTimeBeforePeriodEnd : Bool
    	PeriodActualExpense     : Float
    	PeriodRealisedReturn 	: Float
    end

    data EquityParam extends Param
    	PAL_asset         : Float
    end

    /* Groups */
    data Group extends BaseEntity
        Result : GroupResult
    	Assets   : List<Asset>
    	Policies : List<Policy>
    end

    data GroupResult extends Result
    	IsCalculationEndTimeBeforePeriodEnd : Bool
    	AccumulatedReserveAtEnd             : Float
    	PeriodRealisedReturn                : Float
    	PeriodActualExpense                 : Float
    	PeriodRealisedReturn                : Float
    end

    data ReserveGroup extends Group
        Input    : ReserveInput
        Param    : Param
        Reserve  : Float, Output as CashFlow (Debug, Average)
    end

    data ReserveInput extends Input
    	Reserve : Float
    end

    data Interest extends ReserveGroup
    	DepositRate 						: Float
        SurrenderCharge                     : Float
    end

    data Risk extends ReserveGroup
        RiskDividend 							 : Float
    end

    data Expense extends ReserveGroup
    	TechnicalExpense                 : Expenses
        ExpenseDividend                  : Expenses
    end

    data Expenses
    	CashFlowExpense  : Float
    	ReserveExpense   : Float
    	PolicyFee        : Float
    	SurrenderFee     : Float
    	FreePolicyFee    : Float
    end

    data MarketRateInterest extends Group
    end

    /* Policy */
    data Policy extends BaseEntity
    	Result    : PolicyResult
    	Input     : PolicyInput
    	Groups    : List<Group>
    	CashFlows : List<CashFlow>
    	Equity    : Equity
    	Reserve   : Float
    end

    data PolicyInput extends Input
    	CalculationEndTime : Float
    end

    data PolicyResult extends Result
    	AccumulatedReserveAtEnd     		: Float
    	PeriodCashFlow              		: Float
    	PeriodTechnicalExpenses     		: Expenses
    	PeriodRiskContributionPerGroup  	: Map<String, Float>
    	IsCalculationEndTimeBeforePeriodEnd : Bool
    end

    /* CashFlows */
    data Transfers
    	Amounts : List<Float>
    	Times   : List<Float>
    end

    data CashFlow extends BaseEntity
    	Input   : CashFlowInput
    	Policy  : Policy
    	Transfers 				 : Transfers
    	ScalingFactor 			 : Float
    end

    data CashFlowInput extends Input
    	Transfers 			: Transfers
    end

    data WithExpenses extends CashFlow
        InterestRate : Float
        RiskContributions        : Map<Risk, IFunction>
    end
    data WithoutExpenses extends CashFlow
        InterestRate : Float
        RiskContributions        : Map<Risk, IFunction>
    end
    data ActualExpense extends CashFlow
    end
    data PolicyPaidExpense extends CashFlow
        RiskContributions        : Map<Risk, IFunction>
    end

    /* Global */
    data Global extends BaseEntity
        Input : GlobalInput
        Param : GlobalParam
        BiometricScenarioTimes  : List<Float>
    	OutputDiscountFactor    : Func<Float, Float>
    	ProjectionTimes         : List<Float>
    end

    data GlobalParam extends Param
        DiscountFactorName : String
    end

    data GlobalInput extends Input
        BiometricScenarioTimes 	   : List<Float>
        EconomicScenarioTimes      : List<Float>
    	BiometricScenario 	   : Func<Float, Func<Float, Float>>
    	DiscountFactors   	   : Map<String, Func<Float, Func<Float, Float>>>
    	RealisedDiscountFactors  : Map<String, Func<Float, Func<Float, Float>>>
    end

    fun AllExpenses (value : Float) =
    	new Expenses
    	{	CashFlowExpense = value
    	,	FreePolicyFee = value
    	,   SurrenderFee = value
    	,   PolicyFee = value
    	,   ReserveExpense = value
    	}
     
    init
        Global.ProjectionTimes = skip(1, Global.Input.EconomicScenarioTimes)
    	Global.OutputDiscountFactor = Global.Input.RealisedDiscountFactors(Global.Param.DiscountFactorName)(1.0)
        let alternateValues =
            map n in range(0, 12)
            with n % 2 - 0.5
            end
        let alternatingFunction = createPiecewiseConstant(new Transfers{ Times = range(0,12) , Amounts = alternateValues })
        update policy in Policies
        with
            policy.Reserve = 0
            let riskGroups = policy.Groups:ReserveGroup:Risk
            update cashFlow in policy.CashFlows
            with
                cashFlow.ScalingFactor = 0.5
            end 
            update cashFlow in policy.CashFlows:{WithExpenses, WithoutExpenses, PolicyPaidExpense} with
                let riskContributions =
                    map riskGroup in riskGroups
                    with
                        alternatingFunction
                    end
                cashFlow.RiskContributions = createMap(riskGroups, riskContributions)
            end
        end
        update group in Groups
        with
            update asset in group.Assets
            with
                asset.Weight = asset.Input.Weight
            end
        end
        update reserveGroup in Groups:ReserveGroup
        with
            reserveGroup.Reserve = reserveGroup.Input.Reserve
        end

        update interestGroup in Groups:Interest
        with
            interestGroup.DepositRate = 0.05
            interestGroup.SurrenderCharge = 0
        end
        update riskGroup in Groups:Risk
        with
            riskGroup.RiskDividend = 0.05
        end
        update expenseGroup in Groups:Expense
        with
            expenseGroup.TechnicalExpense = AllExpenses(0.5)
            expenseGroup.ExpenseDividend = AllExpenses(0.5)
        end
        update equity in Equities with
            equity.PremiumScale = 0.5
            equity.BenefitScale = 0.5
            equity.Reserve = 0.0
        end
    end

    manage
        update policy in Policies
        with
            policy.Reserve = policy.Result.AccumulatedReserveAtEnd
        end
        update group in Groups:{Expense, Interest, Risk}
        with
            group.Reserve = group.Result..AccumulatedReserveAtEnd
        end
        update equity in Equities
        with
            equity.Reserve = equity.Result.AccumulatedReserveAtEnd
        end
    end

