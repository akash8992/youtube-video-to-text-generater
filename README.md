Here’s a step-by-step blog post explaining how to extract YouTube video transcripts and save them as text files with the video title as the filename. The blog includes the script, explanations, and instructions for implementation.

---

# How to Extract YouTube Transcripts and Save Them as Text Files Using Python

Do you want to extract transcripts from multiple YouTube videos and save them as text files with the video title as the filename? In this blog, I’ll walk you through a step-by-step process to achieve this using Python. By the end, you’ll have a script that can handle multiple YouTube video links and save their transcripts in an organized way.

---

## Why Extract YouTube Transcripts?

YouTube transcripts are useful for:
- Creating subtitles for videos.
- Analyzing video content for research or SEO purposes.
- Generating summaries or notes from video content.
- Improving accessibility for viewers.

With Python, you can automate the process of extracting transcripts and saving them as text files, making it easier to work with video content programmatically.

---

## Tools and Libraries Used

We’ll use the following Python libraries:
1. **`youtube_transcript_api`**: To fetch YouTube video transcripts.
2. **`pytube`**: To extract video titles.
3. **`re`**: To clean video titles for valid filenames.

Make sure you have these libraries installed before running the script. You can install them using pip:

```bash
pip install youtube-transcript-api pytube
```

---

## Step-by-Step Process

### Step 1: Extract Video ID from YouTube URL

YouTube URLs contain a unique video ID (e.g., `qtkWHhikLh8` in `https://www.youtube.com/watch?v=qtkWHhikLh8`). We’ll use a regular expression to extract this ID.

```python
import re

def get_video_id(url):
    """Extracts the video ID from a YouTube URL."""
    match = re.search(r"(?:v=|\/)([0-9A-Za-z_-]{11}).*", url)
    return match.group(1) if match else None
```

---

### Step 2: Fetch Transcript and Video Title

We’ll use the `youtube_transcript_api` to fetch the transcript and `pytube` to get the video title. The video title will be used as the filename for the transcript.

```python
from youtube_transcript_api import YouTubeTranscriptApi
from pytube import YouTube

def get_youtube_transcript(url, language='en'):
    video_id = get_video_id(url)
    if not video_id:
        print("Invalid YouTube URL")
        return
    
    try:
        # Fetch the video title using pytube
        yt = YouTube(url)
        video_title = yt.title
        
        # Clean the title to make it a valid filename
        video_title = re.sub(r'[\\/*?:"<>|]', "", video_title)
        
        # Fetch the transcript
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
        text = '\n'.join([entry['text'] for entry in transcript])
        
        # Save to a text file with the video title as the filename
        with open(f"{video_title}.txt", "w", encoding="utf-8") as f:
            f.write(text)
        
        print(f"Transcript saved as {video_title}.txt")
    except Exception as e:
        print("Error:", e)
```

---

### Step 3: Process Multiple Video URLs

To process multiple videos, create a list of YouTube URLs and loop through them:

```python
# Example usage with multiple video URLs
video_urls = [
    "https://www.youtube.com/watch?v=qtkWHhikLh8",  # Replace with your first video URL
    "https://www.youtube.com/watch?v=dQw4w9WgXcQ",  # Replace with your second video URL
    # Add more video URLs here as needed
]

for url in video_urls:
    get_youtube_transcript(url)
```

---

### Step 4: Run the Script

Save the script as `youtube_transcript_extractor.py` and run it:

```bash
python youtube_transcript_extractor.py
```

---

## Example Output

If the video titles are:
1. "Introduction to Python Programming"
2. "Advanced Python Techniques"
3. "Python for Data Science"

The script will generate the following files:
- `Introduction to Python Programming.txt`
- `Advanced Python Techniques.txt`
- `Python for Data Science.txt`

Each file will contain the transcript of the corresponding video.

---

## Full Script

Here’s the complete script for easy reference:

```python
from youtube_transcript_api import YouTubeTranscriptApi
from pytube import YouTube
import re

def get_video_id(url):
    """Extracts the video ID from a YouTube URL."""
    match = re.search(r"(?:v=|\/)([0-9A-Za-z_-]{11}).*", url)
    return match.group(1) if match else None

def get_youtube_transcript(url, language='en'):
    video_id = get_video_id(url)
    if not video_id:
        print("Invalid YouTube URL")
        return
    
    try:
        # Fetch the video title using pytube
        yt = YouTube(url)
        video_title = yt.title
        
        # Clean the title to make it a valid filename
        video_title = re.sub(r'[\\/*?:"<>|]', "", video_title)
        
        # Fetch the transcript
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
        text = '\n'.join([entry['text'] for entry in transcript])
        
        # Save to a text file with the video title as the filename
        with open(f"{video_title}.txt", "w", encoding="utf-8") as f:
            f.write(text)
        
        print(f"Transcript saved as {video_title}.txt")
    except Exception as e:
        print("Error:", e)

# Example usage with multiple video URLs
video_urls = [
    "https://www.youtube.com/watch?v=qtkWHhikLh8",  # Replace with your first video URL
    "https://www.youtube.com/watch?v=dQw4w9WgXcQ",  # Replace with your second video URL
    # Add more video URLs here as needed
]

for url in video_urls:
    get_youtube_transcript(url)
```

---

## Conclusion

With this script, you can easily extract transcripts from multiple YouTube videos and save them as text files named after the video titles. This is a great way to automate the process of working with video content, whether for research, accessibility, or content creation.

Feel free to modify the script to suit your needs, such as adding support for multiple languages or customizing the output format. Happy coding! 🚀

---

Let me know if you need further assistance! 😊
