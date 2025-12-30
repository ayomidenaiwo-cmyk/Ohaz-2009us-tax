# 2009 U.S. Federal Personal Income Tax Calculator
# Based on the provided tax table for tax year 2009

# Tax brackets: list of lists for each filing status
# Each bracket: [lower_bound, upper_bound, rate]
# upper_bound = None for the highest bracket ("and above")
brackets = [
    # 0: Single
    [
        [0, 8350, 0.10],
        [8351, 33950, 0.15],
        [33951, 82250, 0.25],
        [82251, 171550, 0.28],
        [171551, 372950, 0.33],
        [372951, None, 0.35]
    ],
    # 1: Married Filing Jointly or Qualifying Widow(er)
    [
        [0, 16700, 0.10],
        [16701, 67900, 0.15],
        [67901, 137050, 0.25],
        [137051, 208850, 0.28],
        [208851, 372950, 0.33],
        [372951, None, 0.35]
    ],
    # 2: Married Filing Separately
    [
        [0, 8350, 0.10],
        [8351, 33950, 0.15],
        [33951, 68525, 0.25],
        [68526, 104425, 0.28],
        [104426, 186475, 0.33],
        [186476, None, 0.35]
    ],
    # 3: Head of Household
    [
        [0, 11950, 0.10],
        [11951, 45500, 0.15],
        [45501, 117450, 0.25],
        [117451, 190200, 0.28],
        [190201, 372950, 0.33],
        [372951, None, 0.35]
    ]
]

status_names = [
    "Single",
    "Married Filing Jointly or Qualifying Widow(er)",
    "Married Filing Separately",
    "Head of Household"
]

def calculate_tax(status: int, income: float) -> float:
    """Calculate tax based on 2009 brackets."""
    if status < 0 or status > 3:
        raise ValueError("Filing status must be 0-3")
    if income < 0:
        raise ValueError("Taxable income cannot be negative")
    
    tax = 0.0
    bracket_list = brackets[status]
    
    for lower, upper, rate in bracket_list:
        if income <= lower:
            break
        
        if upper is None:
            # Highest bracket: tax the rest
            tax += (income - lower) * rate
            break
        else:
            # Tax the portion in this bracket
            bracket_top = min(income, upper)
            tax += (bracket_top - lower) * rate
    
    return round(tax, 2)  # Round to nearest cent

def main():
    print("2009 U.S. Federal Income Tax Calculator\n")
    
    print("Filing statuses:")
    for i, name in enumerate(status_names):
        print(f"{i} - {name}")
    
    try:
        status = int(input("\nEnter filing status (0-3): "))
        income = float(input("Enter taxable income: $"))
        
        tax = calculate_tax(status, income)
        
        print("\n--- Result ---")
        print(f"Filing Status: {status_names[status]}")
        print(f"Taxable Income: ${income:,.2f}")
        print(f"Total Tax Owed: ${tax:,.2f}")
        
    except ValueError as e:
        print(f"Input Error: {e}")
    except Exception:
        print("An unexpected error occurred. Please try again.")

if __name__ == "__main__":
    main()
[Visit GitHub](https://github.com/ifeanyi-us-tax-assignment)
