import os
import csv


csvpath = r"Resources\election_data.csv"

#Open and Read CSV File
with open(csvpath, "r") as csvfile:
    csvreader = csv.reader(csvfile, delimiter=',')
    
    #Setting total votes variable to zero
    totalVotes = 0
    #Dictionary for counting candidate votes
    candidateDict = {}

    # Read each row of data after the header
    for row in csvreader:        
        #Total votes calculation       
        totalVotes +=1

        #These are the headers for the columns A-C
        #row[0] = Voter ID
        #row[1] = County
        #row[2] = Candidate
        
        candidate = row[2]
        if candidate in candidateDict.keys():
            candidateDict[candidate] +=1
        else:
            candidateDict[candidate] = 1

#Declaring a winner   
winner = max(candidateDict, key=candidateDict.get)

# save candidate's percentages in a new dictionary
percentDict = {}
for key in candidateDict.keys():
    percentage = candidateDict[key] / totalVotes
    percentDict[key] = percentage

# Each candidate's data
listOfStrings = []
for key in percentDict.keys():
    myString = key + ": " + str(round(percentDict[key]*100,3)) + "% (" + str(candidateDict[key]) + ")"
    listOfStrings.append(myString)

finalString = "\n".join(listOfStrings)

#Text output
summaryTable = f"""Election Results
-------------------------
Total Votes: {totalVotes}
-------------------------
{finalString}
-------------------------
Winner: {winner}
-------------------------"""
print(summaryTable)
#write to text file
with open("poll_results.txt", "w") as file1:
    file1.write(summaryTable)