# CSV Member Data Transformer

A lightweight, browser-based utility designed for **Sheng Hong Active Ageing Centre (CARE)** to transform membership data for community outreach and reporting. This tool processes data locally, ensuring no sensitive information ever leaves your computer.

## 🚀 Deployment Instructions (GitHub Pages)

To host this tool for free on GitHub:

1.  **Create a Repository:** Create a new repository on GitHub (e.g., `member-transformer`).
2.  **Upload File:** Upload your `index.html` file to the root of the repository.
3.  **Enable Pages:**
    * Go to **Settings** > **Pages**.
    * Set **Source** to "Deploy from a branch".
    * Select the **main** branch and **/(root)** folder.
    * Click **Save**.
4.  **Access:** Wait ~60 seconds. Your app will be live at `https://[your-username].github.io/member-transformer/`.

## 🛠 Features & Logic

The application performs specific data cleaning tasks required for outreach logistics:

* **NRIC Address Splitting:** * Takes the `NricAddress_UnitNo` field (e.g., `11-03`).
    * Creates a **Floor** column (text before the `-`).
    * Creates a **UnitNo** column (text after the `-`).
* **Street Name Extraction:**
    * Parses the `FullAddress` field.
    * Identifies the **StreetName** as the text situated between the first and second comma.
* **Data Integrity:** All other original fields from the source CSV are preserved in the output.
* **Interactive UI:**
    * Modern, simple interface with ghost name (placeholder) support for custom filenames.
    * **Visual Feedback:** The interface turns green and updates icons once a file is successfully uploaded.
    * **Safety Lock:** The "Transform" button is disabled until a file is selected to prevent errors.

## 📋 CSV Input Requirements

The tool is optimized for the standard AAC Member Data format. It specifically looks for:
* `NricAddress_UnitNo`: Used for floor/unit splitting.
* `FullAddress`: Used for street name extraction.

## 🔒 Privacy & Security

* **Client-Side Processing:** This application uses JavaScript (PapaParse) to handle data entirely inside your web browser.
* **No Data Transmission:** No data is sent to a server, database, or third party.
* **Local Output:** The transformed file is generated locally and prompted as a standard browser download.

---
*Created for Sheng Hong Active Ageing Centre (CARE) Outreach Initiatives.*
README.md
Displaying README.md.
