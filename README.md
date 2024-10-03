# WhatsApp Chat Analysis and Visualization

## Project Overview
This project provides an in-depth analysis of a WhatsApp chat dataset, exploring various insights such as emoji usage, word frequency, sentiment analysis, and message patterns by date, hour, and day. Multiple visualizations are created to enhance the understanding of the chat's dynamics. The analysis uses libraries such as `pandas`, `plotly`, `nltk`, `emot`, and `emoji`.

## Features
- **Data Cleaning and Preprocessing:** Messages are cleaned by removing media placeholders, URLs, and non-alphabetic characters.
- **Emoji and Emoticon Extraction:** Emojis and emoticons are extracted, replaced, and analyzed in the dataset.
- **Sentiment Analysis:** Each message is classified into Positive, Negative, or Neutral categories using the VADER Sentiment Analysis tool from NLTK.
- **Message Analysis by Time:** Visualizations explore chat activity trends by hour, day of the week, and month.
- **Most Frequent Words:** A word frequency analysis is performed to identify the top words used in the chat.
- **User and Emoji Analysis:** Top users and their emoji usage patterns are visualized.

## Table of Contents
1. [Installation](#installation)
2. [Libraries Used](#libraries-used)
3. [Dataset](#dataset)
4. [Data Preprocessing](#data-preprocessing)
5. [Analysis and Visualizations](#analysis-and-visualizations)
   - [Emoji and Emoticon Analysis](#emoji-and-emoticon-analysis)
   - [Sentiment Analysis](#sentiment-analysis)
   - [Message Analysis by Time](#message-analysis-by-time)
   - [Word Frequency Analysis](#word-frequency-analysis)
   - [User Analysis](#user-analysis)
6. [Conclusions](#conclusions)
7. [License](#license)

## Installation

### Step 1: Clone the Repository
Clone this repository to your local machine using:
```bash
git clone https://github.com/Lawal0abdulazeez/Whatsapp-Group-Chat-Analysis-sentiment-.git
```

### Step 2: Install Required Libraries
You can install the required libraries using `pip`. Use the following command:
```bash
pip install -r requirements.txt
```

### Step 3: Download NLTK Data
For sentiment analysis, the `VADER` lexicon from NLTK is required. You can download it using the following command:
```python
import nltk
nltk.download('vader_lexicon')
```

### Step 4: Run the Script
Once the dependencies are installed, you can run the notebook or the Python script to see the results.

## Libraries Used

- **pandas**: Data manipulation and analysis.
- **plotly**: Interactive visualizations.
- **nltk**: Sentiment analysis using VADER.
- **emoji & emot**: Emoji and emoticon extraction.
- **stylecloud**: Word cloud generation in custom styles.
- **whatstk**: WhatsApp chat analysis utilities.

## Dataset
The dataset used in this analysis is a WhatsApp chat exported in text format (`WhatsApp_Chat.txt`). It contains timestamps, user names, and messages. The dataset is loaded using the `whatstk` library.

## Data Preprocessing

1. **Clean Text**: Messages are cleaned by removing `<Media omitted>`, deleted messages, URLs, numbers, and non-alphanumeric characters. 
2. **Date and Time Adjustments**: The timestamp of each message is adjusted by shifting all times back by one hour to account for UTC+7.
3. **Text Lowercasing**: All text is converted to lowercase for consistent analysis.

```python
def clean_text(text):
    text = text.replace('<Media omitted>', '').replace('This message was deleted', '').replace('\n', ' ').strip()
    text = re.sub(r'http\S+', '', text)
    text = re.sub(r'[0-9]+', '', text)
    text = re.sub(r'\s+', ' ', text)
    text = re.sub(r'[^\w\s]|_', '', text)
    text = re.sub(r'([a-zA-Z])\1\1', '\\1', text)
    return text.lower()
```

## Analysis and Visualizations

### Emoji and Emoticon Analysis
- **Emoji Extraction**: Extracts emojis from the messages and visualizes the ratio of messages with and without emojis using a pie chart.
- **Most Common Emojis**: Counts the most frequently used emojis in the chat and lists the top 20.

```python
emoji_count = pd.DataFrame(Counter([emoji for message in chat['emoji'] for emoji in message]).most_common(), columns=['emoji', 'count'])
```

### Sentiment Analysis
- **VADER Sentiment Analysis**: Each message's sentiment is classified into Positive, Negative, or Neutral using the `SentimentIntensityAnalyzer` from NLTK.
- **Sentiment Distribution**: A pie chart shows the distribution of sentiments across all messages.

```python
sia = SentimentIntensityAnalyzer()
chat['Sentiment_Score'] = chat['clean_msg'].apply(classify_sentiment)
```

### Message Analysis by Time
- **Top Hours**: A bar chart visualizes the hours of the day when most messages are sent.
- **Top Days of the Week**: A bar chart visualizes the days of the week when most messages are sent.
- **Top Months**: A bar chart visualizes the most active months.
- **Heatmaps**: Heatmaps show the distribution of messages by the hour of the day and the day of the week.

```python
pivot = pd.pivot_table(chat, index='hour', columns='day_name', values='message', aggfunc='count').fillna(0)
heatmap = go.Heatmap(z=pivot.values, x=pivot.columns, y=pivot.index, colorscale='Greens')
```

### Word Frequency Analysis
- **Top Words**: The most frequent words in the chat are identified using the `Counter` from the `collections` module.

```python
words = re.findall(r'\b\w+\b', all_messages.lower())
word_counts = Counter(words)
```

### User Analysis
- **User Interventions**: A line chart visualizes the frequency of each user's interventions in the chat over time.

```python
fig = FigureBuilder(chat).user_interventions_count_linechart()
```

## Conclusions
This project provides a comprehensive analysis of WhatsApp chat data, revealing insights into user behavior, message patterns, and sentiment. It highlights the potential for using text data from chats to understand communication dynamics within groups.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.