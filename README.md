# CSV Member Data Transformer

A lightweight, browser-based utility designed for **Sheng Hong Active Ageing Centre (CARE)** to transform membership data for community outreach and reporting. This tool processes data locally, ensuring no sensitive information ever leaves your computer.

## 🚀 Deployment Instructions (GitHub Pages)

To host this tool for free on GitHub:

1. **Create a Repository:** Create a new repository on GitHub (e.g., `member-transformer`).
2. **Upload File:** Upload your `index.html` file to the root of the repository.
3. **Enable Pages:**
   * Go to **Settings** > **Pages**.
   * Set **Source** to "Deploy from a branch".
   * Select the **main** branch and **/(root)** folder.
   * Click **Save**.
4. **Access:** Wait ~60 seconds. Your app will be live at `https://[your-username].github.io/member-transformer/`.

## 🛠 Features & Logic

The application performs specific data cleaning and transformation tasks required for outreach logistics:

### Data Transforms

* **NRIC Address Splitting:**
  * Takes the `NricAddress_UnitNo` field (e.g., `11-03`).
  * Creates a **Floor** column (text before the `-`).
  * Creates a **UnitNo** column (text after the `-`).

* **Street Name Extraction:**
  * Parses the `FullAddress` field.
  * Identifies the **StreetName** as the text between the first and second comma.

* **Phone Number Splitting:**
  * Parses the `Phone` field (e.g., `(H) 62803149, (M) 98781529`).
  * Creates a **Home Phone** column from the digits after `(H)`.
  * Creates a **Mobile Phone** column from the digits after `(M)`.
  * Either or both values may be blank if not present in the source.

### Column Management

* The following source columns are **removed** after their data has been extracted:
  * `NricAddress_UnitNo` → replaced by `Floor` and `UnitNo`
  * `FullAddress` → replaced by `StreetName`
  * `Phone` → replaced by `Home Phone` and `Mobile Phone`
* All remaining original fields are preserved in the output.
* **Output columns are reordered** so that derived columns appear alongside their related source columns:
  * `Home Phone` and `Mobile Phone` appear where `Phone` was (after `Religion`).
  * Address columns are grouped together: `NricAddress_PostalCode` → `NricAddress_BlockNo` → `Floor` → `UnitNo` → `StreetName` → `NricAddress_Ownership`.

### Filters (applied before download)

* **Service Boundary Filter:** Select **All** (default), **Within**, or **Outside** based on the `ResidesWithinServiceBoundaryStr` field. A live record count updates as filters are changed.
* **Age 60 or Older:** Check this option to include only members whose birth year (from `DateOfBirth`) is 60 or more years before the current year.

### Interactive UI

* **Default File Name:** Pre-filled as `Members_ddMMMyyyy` (today's date) on page load. Fully editable.
* **Visual Feedback:** The upload zone turns green with a tick icon once a file is successfully loaded.
* **Live Record Count:** Displays how many records match the current filters (e.g., *"142 of 310 records match current filters"*) after a file is loaded.
* **Safety Lock:** The "Transform & Download" button is disabled until a file is selected.
* **Download Summary:** The status line after download confirms how many records were exported (e.g., *"Downloaded 142 of 310 records"*).

## 📋 CSV Input Requirements

The tool is optimised for the standard **DataListing-AACMemberDetails.csv** export format. It specifically looks for these source columns:

| Column | Used For |
|---|---|
| `NricAddress_UnitNo` | Floor and UnitNo splitting |
| `FullAddress` | StreetName extraction |
| `Phone` | Home Phone and Mobile Phone splitting |
| `DateOfBirth` | Age 60+ filter (DD-Mon-YYYY format) |
| `ResidesWithinServiceBoundaryStr` | Service boundary filter (Yes / No) |

## 🔒 Privacy & Security

* **Client-Side Processing:** This application uses JavaScript (PapaParse) to handle all data entirely inside your web browser.
* **No Data Transmission:** No data is sent to a server, database, or third party at any point.
* **Local Output:** The transformed file is generated locally and downloaded as a standard browser file download.

---
*Created for Sheng Hong Active Ageing Centre (CARE) Outreach Initiatives.*
