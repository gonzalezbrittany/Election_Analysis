# Colorado Board of Elections Analysis

## Election Analysis Overview
A Colorado Board of Elections employee has asked to complete the election audit of a recent local congressional election.

The following analysis was requested
1. Calculate the total number of votes cast
2. Get a complete list of candidates who received votes.
3. Calculate the total number of votes each candidate received
4. Calculate the percentage of votes each candidate won
5. Determine the winner of the election based on popular vote

## Resources
Data Source: election_results.csv
Software: Python 3.6.1, Visual Studio Code, 1.38.1

## Election Analysis Summary
The analysis of the election show that:
* There were 369,711 votes cast in the election.
* The candidates were:
  * Charles Casper Stockham:
  * Diana DeGette
  * Raymon Anthony Doane
* The candidate results were:
  * Charles Casper Sockham received 23.0% of the vote and 85,213 number of votes.
  * Diana DeGette received 73.8% of the vote and 272,892 number of votes.
  * Raymon Anthony Doane received 3.1% of the vote and 11,606 number of votes.
* The winner of the election was:
  * Diana DeGette, who received 73.8% of the vote and 272,892 number of votes.

## County Analysis Overview
After completing the initial analysis, the Colorado Board of Elections employee than asked for more information in regards to county votes.

The following analysis was requested.
1. Get a complete list of counties who had voters participation in election.
2. Calculate the total number of votes from each county
3. Calculate the percentage of votes from each county
4. Determine which county had the most voter participation for election.

## County Analysis Summary
The analysis of the counties show that:
* The counties were:
  * Jefferson county
  * Denver county
  * Arapahoe county
* The county results were:
  * Jefferson county contributed to 10.5% of the total votes and had 38,855 votes.
  * Denver county contributed to 82.8% of the total votes and had 306,055 votes.
  * Arapahoe county contributed to 6.7% of the total votes and had 24,801 votes.
* The county with the largest county turnout:
  * Denver county, who contributed to 82.8% of the total votes and had 306,055 votes.

## Election-Audit Summary
The code used to analyze the current dataset has been formatted to analysis future election-audit results with a few simple modifications. The first modification would be changing the pathway to the path that contains the new dataset needing to be analyzed, if needed, and update the file name so it matches the new dataset name. The line of code needing to be updated is displayed below.

![image](https://user-images.githubusercontent.com/26393180/149634301-cf306298-df7a-4734-b826-8ae2766fcc8b.png)

The second modification would be to change the pathway for where the results will be saved, if needed, as well as creating a new file name for the new dataset that is being analyzed. This new file will present the analysis results for the new dataset. The line of code needing to be updated is displayed below as well.

![image](https://user-images.githubusercontent.com/26393180/149634310-ab1bdb9b-5d35-46a5-b206-8df666d2a9d8.png)


# Visual Studio Code Results
![image](https://user-images.githubusercontent.com/26393180/149633551-9241eb12-70ab-4a0f-a14b-0d83fd4b6e56.png)

# Visual Studio Analysis Code
    #Add our dependencies.
    import csv
    import os

    #Add a variable to load a file from a path.
    file_to_load = os.path.join("Resources", "election_results.csv")
    #Add a variable to save the file to a path.
    file_to_save = os.path.join("analysis", "election_results.txt")

    #Initialize a total vote counter.
    total_votes = 0
    totalcounty_votes = 0

    #Candidate Options and candidate votes.
    candidate_options = []
    candidate_votes = {}

    #1: Create a county list and county votes dictionary.
    county_options = []
    county_votes = {}


    #Track the winning candidate, vote count and percentage
    winning_candidate = ""
    winning_count = 0
    winning_percentage = 0

    #2: Track the largest county and county voter turnout.
    county_turnout_largest = ""
    county_turnout_count = 0
    county_turnout_percentage = 0


    #Read the csv and convert it into a list of dictionaries
    with open(file_to_load) as election_data:
      reader = csv.reader(election_data)

      # Read the header
      header = next(reader)

      # For each row in the CSV file.
      for row in reader:

          # Add to the total vote count
          total_votes = total_votes + 1
          totalcounty_votes = totalcounty_votes + 1

          # Get the candidate name from each row.
          candidate_name = row[2]

          # 3: Extract the county name from each row.
          county_name = row[1]

          # If the candidate does not match any existing candidate add it to
          # the candidate list
          if candidate_name not in candidate_options:

              # Add the candidate name to the candidate list.
              candidate_options.append(candidate_name)

              # And begin tracking that candidate's voter count.
              candidate_votes[candidate_name] = 0

          # Add a vote to that candidate's count
          candidate_votes[candidate_name] += 1

          # 4a: Write an if statement that checks that the
          # county does not match any existing county in the county list.
          if county_name not in county_options:

              # 4b: Add the existing county to the list of counties.
              county_options.append(county_name)

              # 4c: Begin tracking the county's vote count.
              county_votes[county_name] = 0

          # 5: Add a vote to that county's vote count.
          county_votes[county_name] += 1


    #Save the results to our text file.
    with open(file_to_save, "w") as txt_file:

      # Print the final vote count (to terminal)
      election_results = (
          f"\nElection Results\n"
          f"-------------------------\n"
          f"Total Votes: {total_votes:,}\n"
          f"-------------------------\n\n"
          f"County Votes:\n")
      print(election_results, end="")

      txt_file.write(election_results)

      # 6a: Write a for loop to get the county from the county dictionary.
      for county_name in county_votes:
          # 6b: Retrieve the county vote count.
          countyvotes = county_votes.get(county_name)
          # 6c: Calculate the percentage of votes for the county.
          countyvote_percentage = float(countyvotes) / float(totalcounty_votes) * 100

          # 6d: Print print the current county, its percentage of the total votes and its
          #total votes to  the command line
          county_results = (
              f"{county_name}: {countyvote_percentage:.1f}% ({countyvotes:,})\n") 
          print(county_results)

           # 6e: Save the county votes to a text file.
          txt_file.write(county_results)
           # 6f: Write an if statement to determine the winning county and get its vote count.
          if (countyvotes > county_turnout_count) and (countyvote_percentage > county_turnout_percentage):
              county_turnout_count = countyvotes
              county_turnout_largest = county_name
              county_turnout_percentage = countyvote_percentage

      # 7: Print the county with the largest turnout to the terminal.
      largest_county_summary = (
          f" \n"
          f"-------------------------\n"
          f"Largest County Turnout: {county_turnout_largest}\n"
          f"-------------------------\n")
      print(largest_county_summary)

      # 8: Save the county with the largest turnout to a text file.
      txt_file.write(largest_county_summary)

      # Save the final candidate vote count to the text file.
      for candidate_name in candidate_votes:

          # Retrieve vote count and percentage
          votes = candidate_votes.get(candidate_name)
          vote_percentage = float(votes) / float(total_votes) * 100
          candidate_results = (
              f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

          # Print each candidate's voter count and percentage to the
          # terminal.
          print(candidate_results)
          #  Save the candidate results to our text file.
          txt_file.write(candidate_results)

          # Determine winning vote count, winning percentage, and candidate.
          if (votes > winning_count) and (vote_percentage > winning_percentage):
              winning_count = votes
              winning_candidate = candidate_name
              winning_percentage = vote_percentage

      # Print the winning candidate (to terminal)
      winning_candidate_summary = (
          f"-------------------------\n"
          f"Winner: {winning_candidate}\n"
          f"Winning Vote Count: {winning_count:,}\n"
          f"Winning Percentage: {winning_percentage:.1f}%\n"
          f"-------------------------\n")
      print(winning_candidate_summary)

      # Save the winning candidate's name to the text file
      txt_file.write(winning_candidate_summary)
