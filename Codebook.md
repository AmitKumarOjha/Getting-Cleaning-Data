1. Subject - The variable which identifyies the 30 people on whom the experiments were performed
2. Activity_Description - A variable which describes the activity performed by the individual subject. This is obtained by merging the consoldated data (training + test) with activity labels using the common key as Activity Id.
3. Activity_ID - The Variable denoting the activity id for each activity
4. Other 79 columns - Mean of each measurement grouped by Subject and Activity. Measurments for creating the finally grouped tidy data set were extracted from overall 561 featured by matching the strings mean or std. There 79 columns are further cleaned by removing parenthesis.
