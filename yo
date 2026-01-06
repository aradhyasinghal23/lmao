import streamlit as st
import os
from markitdown import MarkItDown
from requests import Session

# --- App Configuration ---
st.set_page_config(page_title="Universal Document Reader", page_icon="📄")

def main():
    st.title("📄 Universal Document Reader")
    st.markdown("Convert Office docs, PDFs, and HTML into clean Markdown instantly.")

    # --- Initialize Engine & Session ---
    # Custom session for stability as per requirements
    session = Session()
    session.headers.update({"User-Agent": "UniversalDocReader/1.0"})
    
    # Initialize MarkItDown with a 5-second timeout
    md_engine = MarkItDown(requests_session=session)

    # --- Upload Area ---
    uploaded_files = st.file_uploader(
        "Drag and drop files here", 
        type=["docx", "xlsx", "pptx", "pdf", "html"],
        accept_multiple_files=True
    )

    if uploaded_files:
        for uploaded_file in uploaded_files:
            file_name = uploaded_file.name
            base_name, _ = os.path.splitext(file_name)
            
            try:
                # To process with MarkItDown, we save temporarily or pass the stream
                # MarkItDown works best with file paths or bytes
                with st.spinner(f"Processing {file_name}..."):
                    # We pass the file object directly to MarkItDown
                    result = md_engine.convert(uploaded_file)
                    converted_text = result.text_content

                # --- Instant Preview ---
                with st.expander(f"👁️ Preview: {file_name}", expanded=True):
                    st.text_area(
                        label="Converted Content",
                        value=converted_text,
                        height=300,
                        key=f"preview_{file_name}"
                    )

                    # --- Download Options ---
                    col1, col2 = st.columns(2)
                    
                    with col1:
                        st.download_button(
                            label="Download as .md",
                            data=converted_text,
                            file_name=f"{base_name}_converted.md",
                            mime="text/markdown",
                            key=f"md_{file_name}"
                        )
                    
                    with col2:
                        st.download_button(
                            label="Download as .txt",
                            data=converted_text,
                            file_name=f"{base_name}_converted.txt",
                            mime="text/plain",
                            key=f"txt_{file_name}"
                        )

            except Exception as e:
                st.error(f"⚠️ Could not read {file_name}. Please check the format.")
                # Optional: Log the error for debugging
                # st.info(f"Technical details: {e}")

if __name__ == "__main__":
    main()
