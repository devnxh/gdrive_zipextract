# gdrive_zipextract
# Google Drive ZIP Extractor for Colab

**Extract ZIP files directly inside Google Drive — no download, no re-upload.**

This tool is a Jupyter Notebook (`gdrive_unzip_colab.ipynb`) designed to run on Google Colab. It mounts your Google Drive, finds `.zip` files in the folders you specify, and extracts them directly back into your Drive. It is highly optimized for large files (tens or hundreds of GB), streaming them member-by-member to keep local disk and RAM usage minimal.

## Features

- **Direct Extraction**: Extracts ZIP files inside Google Drive without downloading to your local machine.
- **Resource Efficient**: Uses a CPU runtime (cheapest option) and requires minimal RAM/disk space.
- **Resumable**: If the runtime disconnects or stops, simply run it again — it will resume exactly where it left off.
- **Atomic Writes**: Uses an atomic writing pattern (`*.part` → verify → rename) ensuring no half-written files are left behind.
- **Fallback Support**: Can handle Deflate64 and AES-encrypted ZIP files using `7z` and `pyzipper` fallbacks.

## Quick Start Guide

Follow these steps to use the notebook in Google Colab:

1. **Open the Notebook in Colab:**
   Upload the `gdrive_unzip_colab.ipynb` file to your Google Drive and double-click it to open it with Google Colaboratory.

2. **Configure Your Settings:**
   In the first code cell (labeled **🔧 Configuration**), set your desired options:
   - **`SOURCE_PATHS`**: (Required) Provide a semicolon-separated list of Drive folders or individual `.zip` files you want to extract (e.g., `/content/drive/MyDrive/ZIPS`).
   - **`DEST_MODE`**: Choose where to extract the files (`folder_next_to_zip`, `same_folder`, or `custom`).
   - **`ON_EXISTING`**: Choose how to handle existing files (`skip_if_same_size`, `overwrite`, or `keep_both`).
   - **`WORKERS` / `CHUNK_MB`**: Adjust performance parameters if needed (defaults are usually fine).

3. **Set Runtime to CPU:**
   Go to the top menu and click **Runtime → Change runtime type**. Ensure the hardware accelerator is set to **CPU** (GPUs/TPUs offer no benefit for this task).

4. **Run the Notebook:**
   Go to **Runtime → Run all** (or press `Ctrl+F9`).

5. **Authorize Google Drive:**
   A prompt will appear asking for permission to access your Google Drive. Click to authorize and allow the notebook to mount your Drive.

6. **Wait for Completion:**
   Keep the browser tab open and your computer awake until the final extraction report is printed at the end of the notebook. 
   
> **Note:** If the notebook stops or the session disconnects before finishing, simply click **Run all** again. The script uses a small state file and will pick up right where it stopped!

## Additional Configuration

- **Encryption**: If your ZIP is encrypted, leave the `PASSWORD` field blank. The script will safely prompt you for the password via a secure input field when it encounters an encrypted file.
- **Testing**: For a dry run without modifying files, set `DRY_RUN = True`.
