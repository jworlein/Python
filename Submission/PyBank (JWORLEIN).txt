import os
import csv

csvpath = r"Resources\budget_data.csv"
#Open and Read CSV File
with open(csvpath, "r") as csvfile:
    csvreader = csv.reader(csvfile, delimiter=',')
    #Skip CSV Header Row
    csv_header = next(csvreader)
    
    # Variable Assignments and Definitions
    months = []
    profit_loss_changes = []

    count_months = 0
    net_profit_loss = 0
    previous_month_profit_loss = 0
    current_month_profit_loss = 0
    profit_loss_change = 0
             
    # Read through each row of data after the header
    for row in csvreader:

    #   Count of months
        count_months += 1

        # Net total amount of profit and losses
        current_month_profit_loss = int(row[1])
        net_profit_loss += current_month_profit_loss

        if (count_months == 1):
            # Previous month to be equal to current month
            previous_month_profit_loss = current_month_profit_loss
            continue

        else:

            # Change in profit/loss 
            profit_loss_change = current_month_profit_loss - previous_month_profit_loss
            # Append each month to the months[]
            months.append(row[0])
            # Append each profit_loss_change to the profit_loss_changes[]
            profit_loss_changes.append(profit_loss_change)
            # Make the current_month_loss to be previous_month_profit_loss for the next iteration of loop
            previous_month_profit_loss = current_month_profit_loss

    #sum and average of profit/loss
    sum_profit_loss = sum(profit_loss_changes)
    average_profit_loss = round(sum_profit_loss/(count_months - 1), 2)

    # highest and lowest changes in profit/loss
    highest_change = max(profit_loss_changes)
    lowest_change = min(profit_loss_changes)

    # Locate the index value of highest and lowest changes in profit/loss
    highest_month_index = profit_loss_changes.index(highest_change)
    lowest_month_index = profit_loss_changes.index(lowest_change)

    # Assign best and worst month
    best_month = months[highest_month_index]
    worst_month = months[lowest_month_index]

summaryTable = f"""Financial Analysis
-------------------------
Total Months:  {count_months}
Total:  ${net_profit_loss}
Average Change:  ${average_profit_loss}
Greatest Increase in Profits:  {best_month} (${highest_change})
Greatest Decrease in Losses:  {worst_month} (${lowest_change})"""

print(summaryTable)
#write summary string
with open("budget_data.txt", "w") as file1:
    file1.write(summaryTable)
