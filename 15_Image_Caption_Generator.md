# Image Caption Generator With Streamlit :
```
import streamlit as st
from openai import OpenAI
from dotenv import load_dotenv
import os

# Load API key from .env
load_dotenv()
client = OpenAI(api_key=os.getenv("OPEN_API_KEY"))

# Streamlit UI
st.set_page_config(page_title="🖼️ Image Caption Generator", layout="centered")
st.title("🖼️ Image Caption Generator")

# Input field for image URL
image_url = st.text_input("Paste Image URL", placeholder="Paste image url here...")

# if image url is correct
if image_url:
    st.image(image_url, caption="Input Image", use_container_width=True)
    
    with st.spinner("Generating caption..."):
        try:
            response = client.chat.completions.create(
                model="gpt-4.1-mini",
                messages=[
                    {
                        "role": "user",
                        "content": [
                            {"type": "text", "text": "Generate a detailed caption for this image in 50 words."},
                            {"type": "image_url", "image_url": {"url": image_url}}
                        ]
                    }
                ],
                max_tokens=100
            )
            caption = response.choices[0].message.content
            st.success("Response :")
            st.markdown(f"**📝 : {caption}**")

        except Exception as e:
            st.error(f"Failed to generate caption: {e}")

```