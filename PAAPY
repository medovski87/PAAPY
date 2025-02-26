import requests
import pandas as pd
import time
import streamlit as st

# Replace with your SerpAPI key
SERPAPI_KEY = "6001040ff638a099c06c735d5446124e2fc35fc3d8b5de59533d76083ad64316"

# Function to get PAA questions
def get_paa_questions(keyword):
    url = "https://serpapi.com/search"
    params = {
        "q": keyword,
        "hl": "en",
        "api_key": SERPAPI_KEY,
    }
    
    response = requests.get(url, params=params)
    data = response.json()
    
    paa_list = []
    
    if "questions" in data:
        for question in data["questions"]:
            paa_list.append({
                "Keyword": keyword,
                "PAA Question": question.get("question", ""),
                "Source URL": question.get("link", "")
            })
    
    return paa_list

# Streamlit UI
st.title("People Also Ask (PAA) Extractor")
st.write("Enter keywords below to extract Google's People Also Ask questions.")

keywords_input = st.text_area("Enter keywords (separated by commas)")
if st.button("Fetch PAA Questions"):
    keywords = [kw.strip() for kw in keywords_input.split(',') if kw.strip()]
    
    if keywords:
        all_paa_data = []
        for kw in keywords:
            st.write(f"Fetching PAA for: {kw}")
            all_paa_data.extend(get_paa_questions(kw))
            time.sleep(2)  # Avoid hitting API rate limits

        # Convert to DataFrame
        paa_df = pd.DataFrame(all_paa_data)
        st.write("### Extracted PAA Questions")
        st.dataframe(paa_df)

        # Download button for CSV
        csv = paa_df.to_csv(index=False).encode('utf-8')
        st.download_button("Download CSV", csv, "paa_questions.csv", "text/csv")
    else:
        st.write("Please enter at least one keyword.")
