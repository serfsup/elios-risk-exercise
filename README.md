# elios-risk-exercise

  This is a practice problem with fake data on risk assessment. It is representative of 25 individuals with 13 columns of data. The columns are labeled:
      addresses
      court
      criminal
      dob
      emails
      fname
      full_name
      lname
      mname
      phone_numbers
      properties
      sec
      social_media

  The first problem faced when accessing the .json file is extracting the data. The file is a list of dictionaries, so creating a data frame with the initial file yields columns, each its own puzzle to decipher. One column ('sec') is an empty list for all instances, and can be deleted. The 'dob', 'phone_numbers', and the four name files are all readable from the start, but all other columns have additional data that must be unpacked. I created individual data frames for each (sometimes up to six separate data frames), dropped columns with no variability, and then combined them all together to make one master data frame.
  
  At this point, a few decisions needed to be made. The most important one has to do with chosing the target for our model. Upon doing a bit of research on traditional risk assessment techniques, I found that instances are often regarded in a matrix combining two factors:  the probability of an event occuring and the severity of the event, should it occur. Using this technique, my thought was to create a feature that deals with the two most important columns of data (in my opinion), 'court' and 'criminal'. The four columns that seem to be the most indicative of risk would include the 'Court_party' column (whether a plaintiff, defendant, or neither), the 'Court_disposition' column (dismissed, discharged, transfered, etc.), the 'Offense_description' column (assault, escape, DUI, etc.), and the 'Offense_disposition' column (found guilty, unsentenced, etc.).

  When converting to categorical data, ranking these four columns from lesser offenses to worse offenses seems to be a logical way to gleen insight from the designations. My thought was by using number rankings encoding the categoricals, a formula can be created to arrive at a risk score for each individual. This score can then be the target value when training a predictive model. The key is coming up with a formula that will effectively score people when they are missing values from any of the four columns. I decided to coerce the NaN values to zero and have all other values begin at 1, so a person who did not have any offenses would earn a 0 for each value, while a minor offense would be 1 or 2 points, and a major offense would be up to 8 points. Then by multiplying the two values together and adding them up, we could gain a risk value. I also determined that repeat offenses should count more, so incorporating a multiplier into the formula would also be beneficial (first offense is x 0.1, third offense is x 0.3).
  
  Once this is completed, the X and y can be set. X = risk_df.drop('risk_score') and y = risk_df['risk_score']. Train-test-split to create training and test data, and then run regressions to see which model fits the best.
  
  There are a few other items of note. 12 out of 22 first offenders were born after their disposition date. I understand that this is fake data, but if this were actual data, something would need to be done about those values in order to run an effective model. Also, every single criminal offense occurred at 575 Maple Street in each respective city across the country. Doesn't sound like a healthy place to visit! There were also some values in the properties columns that I would combine in the addresses, as the house number, street name, street prefix, and street suffix were all separated into columns. Putting them all together as they are listed in the addresses column would help to eliminate variables that could be redundant.
  
  There is so much more that I would do if I had time! This exercise has been extremely satisfying, and one that I hope to continue working on in my spare time.
