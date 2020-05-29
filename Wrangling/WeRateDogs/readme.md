# Data Wrangling of Tweets from @WeRateDogs
This folder stores the process and report of Data Wrangling on tweets and other information related to @WeRateDogs.
Check the analysis and visualizations for these data, please see: [Exploratory Data Analysis on tweets from @WeRateDogs](https://htmlpreview.github.io/?https://github.com/XinyueYu16/Data_Visualizations/blob/master/Insights%20from%20%40WeRateDogs.html)

## Data Source
 related to the three datasets of: WeRateDogs Tweet Archive, Predictions made on WeRateDogs tweets, and like and retweets counts scraped from Tweepy.
 - WeRateDogs Tweet Archive
 - Predictions made on WeRateDogs tweets
 - Like and retweet counts scraped from Tweepy
 
## Data Assesment
### Quality Issues
Before wrangling, I assessed the quality and tidiness issues for these three datasets.

For __quality issues__, I listed 10 points as follows, consisting completeness, validity and consistency issues.

#### Completeness Issues

__Archive Data__
- Thousands of null values in columns of `in_reply_to_status_id`, `in_reply_to_user_id`, `retweeted_status_id`, `retweeted_status_user_id`, `retweeted_status_timestamp`;
- The last 5 columns contains many empty values, but they are shown in strings of 'None';
- Duplicates in `expanded_urls` column;
    
__Prediction Data__
- Duplicates in  `jpg_url` column;

#### Validity issues
__Archive Data__
- The `name` column contain 'a's as name;
- The values of `source` column contain extra tag information of scraped links (i.e. <a href = ...);
- The `rating_denominator` column contains denominators that are not 10;
- The text columns contain URLs;

#### Accuracy issues
__Archive Data__
- The `rating_numerator` column contains values that are way larger than the denominator;
                                                                                        
#### Consistency issues
__Prediction Data__
- Lowercase and uppercase spelling are mixed in predictions.

### Tidiness Issues
For tidiness issues, I listed 2 points: 
                                                                                        
__Archive Data__
- The retweet and in-reply statement shouldn't be in the same table with the dog information;
- The last 4 columns represent 4 values for a variable indicating the dog's age.

## Data Wrangling
During the data wrangling part, I delt with the quality and tidiness issues one by one, the logic and processes of wrangling are shown as follows.
### Quality Issues
#### Completeness Issues
- Empty values: 
    - For columns that contain too many empty values and irrelevant to the analysis like `in_reply_to_status_id` etc., I dropped the columns entirely;
    - For the last 5 columns in archive data, I firstly transformed the strings of 'None's to NaN, and then leave them there. Because we do not have enough information to fill those columns and it would be a waste of information if we dropped those empty rows entirely.
    
- Duplicated values:
    - For duplicated values like values in `archive.expanded_urls` and `pred.jpg_url` columns, I dropped the duplicated rows.
    
#### Validity Issues
- For validity issues, I did following wrangling efforts:
    - Transformed the 'a' in `name` to NaN;
    - Extracted texts for `source` column and cleaned the URLs in `text` column using RegEx;
    - Filtered out rows with `rating_denominator` values that are not 10;

#### Accuracy Issues
- Filtered out rows with `rating_numerator` values that are way larger than the denominator, the criteria was `rating_numerator` values need to be less than or equal to 15;

#### Consistency Issues
- For consistency the simplicity in future data analysis, I changed all the dog types in `pred` to lowercase spellings.

### Tidiness Issues
- Since the last 4 columns of `archive dara` are representing 4 values for a variable indicating the dog's age, I turned these 4 columns into one column called `growth_period`;

- To maintain different observation units in their separate tables, I divided the `tweet archive` into two tables names `dog_rating` and `tweet_info`;

- Further more, I inner joined `tweet_info` with `like and retweet count` of tweets on `tweet_id`, since these two tables are describing the same observational unit. Finally, this dataset was stored as `twitter_archive_master`.

## Conclusion of Data Wrangling
After wrangling, I saved these data into three files, they are:
- twitter_archive_master: storing information about each tweet itself;
- dog-rating: storing description and ratings about each dog; 
- pred: storing predictions made on the dog-ratings.
