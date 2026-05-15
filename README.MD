Gemini
CSV Transformer WebApp Design
New chat
My stuff
Notebooks

PDPA
New notebook
Gems
Chats
CSV Transformer WebApp Design
create image of an asian man and women doing agility ladder exercise. no background
Medical Escort Volunteer Checklist Design
two asian seniors, one man and one woman, exercising. Man using a pair of dumbbell doing lunges with hands and dumbbells pressing overhead, woman using resistance band doing squat and pulling band back, training the back muscle. show full body and cartoon format
Debugging Data Transformation: CFS Missing
update: women on right of slightly darker skin color
Cartoon format. 3 seniors, 2 women and 1 man, all asians, exercising with dumbbells and resistance band. show only the top half of the body. both women slightly angled to the sides and the man behind looking straight, slightly chin up
Bingo App Redesign: Wide Screen
WhatsApp Queue App Filter Update
Mental Health Presentation Structure
UCC Motorcycle Parking Options
I do not like the color, look and feel of this flyer. recreate one that is easy to read for seniors and retain all key information.
I have created a BCRS flyer (attached), but I need to follow the guidelines. Amend the flyer and create the visual
Flyer Amendment Based on Guidelines
Seniors' BCRS Workshop Flyer Creation
Singapore Chinese Translation For Seniors
Calculating Date 60 Days Prior

Settings & help
Conversation with Gemini
[design] create a webapp that run on github (free) to transform attached file and output to a csv locally.



1. Keep UI modern and simple.

2. Prompt for input with ghost name in the field.

3. For field NricAddress_UnitNo, transform into Floor: text before "-" and UnitNo: text after "-".

4. for field FullAddress, extract into StreetName: text between the first and third ",".

5. rest of fields as in original csv.



question?

To create a web application that runs for free on GitHub Pages and performs these specific transformations, you can use a single HTML file with embedded JavaScript. This approach requires no backend and allows for local processing of your CSV data.

Below is the complete code for index.html. You can host this on a GitHub repository and enable "GitHub Pages" in the settings.

The Solution: CSV Transformer WebApp
HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Member Data Transformer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
</head>
<body class="bg-gray-50 min-h-screen flex items-center justify-center p-6">

    <div class="max-w-2xl w-full bg-white rounded-xl shadow-lg p-8 border border-gray-100">
        <header class="mb-8">
            <h1 class="text-2xl font-bold text-gray-800">AAC Member Data Transformer</h1>
            <p class="text-gray-500 text-sm">Upload your member details CSV to apply local transformations.</p>
        </header>

        <div class="space-y-6">
            <div>
                <label class="block text-sm font-medium text-gray-700 mb-2">Project/Batch Name</label>
                <input type="text" id="batchName" 
                       placeholder="e.g. Sennett_Batch_May2026" 
                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all placeholder-gray-300">
            </div>

            <div class="border-2 border-dashed border-gray-200 rounded-xl p-10 flex flex-col items-center justify-center bg-gray-50 hover:bg-gray-100 transition-colors cursor-pointer" id="dropZone">
                <svg class="w-12 h-12 text-gray-400 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path>
                </svg>
                <p class="text-sm text-gray-600 font-medium">Click to upload or drag and drop</p>
                <p class="text-xs text-gray-400 mt-1">DataListing-AACMemberDetails.csv</p>
                <input type="file" id="fileInput" accept=".csv" class="hidden">
            </div>

            <button id="processBtn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition-all shadow-md disabled:opacity-50 disabled:cursor-not-allowed">
                Transform & Download CSV
            </button>

            <div id="status" class="text-center text-sm font-medium hidden"></div>
        </div>

        <footer class="mt-8 pt-6 border-t border-gray-100 text-center">
            <p class="text-xs text-gray-400">Processing is done locally in your browser. No data is sent to a server.</p>
        </footer>
    </div>

    <script>
        const fileInput = document.getElementById('fileInput');
        const dropZone = document.getElementById('dropZone');
        const processBtn = document.getElementById('processBtn');
        const status = document.getElementById('status');

        dropZone.onclick = () => fileInput.click();

        fileInput.onchange = (e) => {
            if (e.target.files.length > 0) {
                status.innerText = `Selected: ${e.target.files[0].name}`;
                status.classList.remove('hidden', 'text-red-500');
                status.classList.add('text-green-600');
            }
        };

        processBtn.onclick = () => {
            const file = fileInput.files[0];
            if (!file) {
                alert("Please select a file first.");
                return;
            }

            Papa.parse(file, {
                header: true,
                skipEmptyLines: true,
                complete: function(results) {
                    const transformedData = results.data.map(row => {
                        // 1. Transform NricAddress_UnitNo -> Floor and UnitNo
                        let floor = "";
                        let unitNo = "";
                        if (row.NricAddress_UnitNo && row.NricAddress_UnitNo.includes('-')) {
                            const parts = row.NricAddress_UnitNo.split('-');
                            floor = parts[0].trim();
                            unitNo = parts[1].trim();
                        }

                        // 2. Transform FullAddress -> StreetName (text between 1st and 3rd comma)
                        let streetName = "";
                        if (row.FullAddress) {
                            const addrParts = row.FullAddress.split(',');
                            // Index 1 is text between 1st/2nd comma, Index 2 is text between 2nd/3rd comma
                            if (addrParts.length >= 3) {
                                streetName = (addrParts[1].trim() + " " + addrParts[2].trim()).trim();
                            } else if (addrParts.length === 2) {
                                streetName = addrParts[1].trim();
                            }
                        }

                        // Return new object with transformations and all original fields
                        return {
                            ...row,
                            Floor: floor,
                            UnitNo: unitNo,
                            StreetName: streetName
                        };
                    });

                    downloadCSV(transformedData);
                }
            });
        };

        function downloadCSV(data) {
            const csv = Papa.unparse(data);
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement("a");
            
            const batch = document.getElementById('batchName').value || 'transformed_data';
            link.setAttribute("href", url);
            link.setAttribute("download", `${batch}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            status.innerText = "Processing Complete! Check your downloads.";
        }
    </script>
</body>
</html>
How to use this:
Create a Repository: Go to GitHub and create a new repository (e.g., aac-transformer).

Upload File: Upload the code above as index.html.

Enable GitHub Pages:

Go to Settings > Pages.

Under Build and deployment > Source, select Deploy from a branch.

Select main branch and /root folder. Save.

Access App: After a minute, your site will be live at https://[your-username].github.io/aac-transformer/.

Transformation Logic Explained:
Unit Number: It takes a value like 11-03, splits it by the hyphen, and assigns 11 to the Floor column and 03 to the UnitNo column.

Street Name: Based on your sample data (1, JALAN TAMAN, 11-03, Singapore 329022), it looks for the text between the 1st and 3rd comma. In this example, it extracts JALAN TAMAN 11-03.

Original Data: All original columns from the source file are preserved in the output.

Question for you:
For the StreetName field, if the address format varies (e.g., fewer than 3 commas), should the app default to a specific part of the address, or is the format strictly consistent as per the provided sample?

update, the street address is between the first and second ","

I have updated the code to extract the StreetName specifically as the text found between the first and second commas of the FullAddress field.

Here is the updated complete index.html file for your GitHub repository:

HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Member Data Transformer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
</head>
<body class="bg-slate-50 min-h-screen flex items-center justify-center p-6">

    <div class="max-w-xl w-full bg-white rounded-2xl shadow-xl p-8 border border-slate-100">
        <header class="mb-10 text-center">
            <h1 class="text-2xl font-bold text-slate-800 tracking-tight">Member Data Transformer</h1>
            <p class="text-slate-500 text-sm mt-2">Local CSV transformation for community outreach</p>
        </header>

        <div class="space-y-6">
            <div>
                <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2 ml-1">Output Filename</label>
                <input type="text" id="batchName" 
                       placeholder="e.g. Outreach_List_May2026" 
                       class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-indigo-500 focus:bg-white outline-none transition-all placeholder-slate-300 text-slate-700">
            </div>

            <div class="group border-2 border-dashed border-slate-200 rounded-2xl p-12 flex flex-col items-center justify-center bg-slate-50 hover:bg-indigo-50 hover:border-indigo-200 transition-all cursor-pointer relative" id="dropZone">
                <div class="bg-white p-3 rounded-full shadow-sm group-hover:scale-110 transition-transform">
                    <svg class="w-8 h-8 text-indigo-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path>
                    </svg>
                </div>
                <p class="text-sm text-slate-600 font-medium mt-4">Drop your member CSV here</p>
                <p class="text-xs text-slate-400 mt-1" id="fileNameDisplay">DataListing-AACMemberDetails.csv</p>
                <input type="file" id="fileInput" accept=".csv" class="hidden">
            </div>

            <button id="processBtn" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 rounded-xl transition-all shadow-lg shadow-indigo-100 active:scale-[0.98]">
                Transform & Download
            </button>

            <div id="status" class="text-center text-sm font-semibold hidden py-2 rounded-lg"></div>
        </div>

        <footer class="mt-10 pt-6 border-t border-slate-50 text-center">
            <p class="text-[10px] text-slate-400 uppercase tracking-widest font-bold">Privacy Guaranteed: Data never leaves your computer</p>
        </footer>
    </div>

    <script>
        const fileInput = document.getElementById('fileInput');
        const dropZone = document.getElementById('dropZone');
        const processBtn = document.getElementById('processBtn');
        const status = document.getElementById('status');
        const fileNameDisplay = document.getElementById('fileNameDisplay');

        dropZone.onclick = () => fileInput.click();

        fileInput.onchange = (e) => {
            if (e.target.files.length > 0) {
                fileNameDisplay.innerText = e.target.files[0].name;
                status.innerText = "File ready for transformation";
                status.className = "text-center text-sm font-semibold py-2 rounded-lg text-indigo-600 bg-indigo-50 block";
            }
        };

        processBtn.onclick = () => {
            const file = fileInput.files[0];
            if (!file) {
                alert("Please select a CSV file first.");
                return;
            }

            Papa.parse(file, {
                header: true,
                skipEmptyLines: true,
                complete: function(results) {
                    const transformedData = results.data.map(row => {
                        // 1. Transform NricAddress_UnitNo (Floor before -, UnitNo after -)
                        let floor = "";
                        let unitNo = "";
                        const unitRaw = row.NricAddress_UnitNo || "";
                        if (unitRaw.includes('-')) {
                            const parts = unitRaw.split('-');
                            floor = parts[0].trim();
                            unitNo = parts[1].trim();
                        }

                        // 2. Transform FullAddress (StreetName is text between 1st and 2nd comma)
                        let streetName = "";
                        const fullAddr = row.FullAddress || "";
                        if (fullAddr.includes(',')) {
                            const addrParts = fullAddr.split(',');
                            if (addrParts.length > 1) {
                                // Index 1 is the content between the first and second comma
                                streetName = addrParts[1].trim();
                            }
                        }

                        // Return combined data: Original fields + transformed new columns
                        return {
                            ...row,
                            Floor: floor,
                            UnitNo: unitNo,
                            StreetName: streetName
                        };
                    });

                    downloadCSV(transformedData);
                }
            });
        };

        function downloadCSV(data) {
            const csv = Papa.unparse(data);
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement("a");
            
            const batch = document.getElementById('batchName').value.trim().replace(/\s+/g, '_') || 'transformed_members';
            link.setAttribute("href", url);
            link.setAttribute("download", `${batch}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            status.innerText = "Success! File downloaded locally.";
            status.className = "text-center text-sm font-semibold py-2 rounded-lg text-green-600 bg-green-50 block";
        }
    </script>
</body>
</html>
Changes implemented:
Logic Change: The StreetName logic now splits the FullAddress by commas and takes only the second element (the text between the 1st and 2nd comma).

UI Tweaks: Kept the "ghost name" (placeholder) in the input field and maintained a clean, modern interface using Tailwind CSS.

Security: This app uses PapaParse to handle the CSV processing entirely inside the user's browser, making it safe and compliant with privacy needs.

Implementation Checklist:
Copy the code above.

Paste it into a file named index.html in your GitHub repo.

Ensure GitHub Pages is pointing to that repo's main branch.

Your app will be ready to transform the DataListing-AACMemberDetails.csv file immediately upon upload.

i get a 404. tell me the steps to deply in github

A 404 error on GitHub Pages usually means the service hasn't finished building your site, or it's looking for a file that isn't where it expects it to be (like a folder mismatch).

Since we are using a single index.html file, follow these exact steps to get it live:

1. Check File Naming
GitHub Pages defaults to looking for a file named exactly index.html (all lowercase) in the root directory.

Ensure your file is not named Index.html or transformer.html.

Ensure it is in the main folder, not inside a subfolder like /scripts.

2. Configure GitHub Pages Settings
Navigate to your repository on GitHub.

Click on the Settings tab (top menu).

On the left sidebar, click Pages.

Under Build and deployment > Branch:

Select main (or master) from the dropdown.

Select /(root) from the second dropdown.

Click Save.

3. Verify the Deployment URL
After clicking Save, look at the top of that same Pages screen.

You will see a yellow or green bar saying "Your site is live at...".

Wait 1–2 minutes. GitHub takes a moment to process the file. If you click it immediately, you might still see the 404. Refresh after a minute.

4. Troubleshooting the URL Structure
If your repository is named member-tool, your URL must look like this:
https://[your-username].github.io/member-tool/

Note: If you are accessing https://[your-username].github.io/, it will 404 because it's missing the repository name at the end.

Summary Checklist
Issue	Solution
Wrong Filename	Rename file to index.html.
Wrong Branch	Ensure "Pages" is set to main.
Empty Repo	Ensure the code I provided is committed and pushed to GitHub.
Delay	Wait 60 seconds for the "Actions" tab to show a green checkmark.
Quick Tip: Click the Actions tab at the top of your GitHub repo. If you see a workflow running (usually called "pages-build-deployment"), wait until it turns green before refreshing your site.

update interface. when file is uploaded, change interafce to green. transform and download button is disabled until file is uploaded.

I have updated the code to include the interactive UI states you requested. The button is now disabled by default, and the upload area will transform into a green success state once a valid CSV is detected.

Here is the updated index.html:

HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Member Data Transformer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <style>
        .drop-zone-active { @apply border-green-500 bg-green-50; }
        .btn-disabled { @apply opacity-50 cursor-not-allowed bg-slate-400 hover:bg-slate-400 shadow-none transform-none; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen flex items-center justify-center p-6 font-sans">

    <div class="max-w-xl w-full bg-white rounded-3xl shadow-2xl p-10 border border-slate-200">
        <header class="mb-10 text-center">
            <h1 class="text-3xl font-black text-slate-800 tracking-tight">CSV Transformer</h1>
            <p class="text-slate-500 text-sm mt-2 font-medium">Local processing for AAC Member Records</p>
        </header>

        <div class="space-y-8">
            <div>
                <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-[0.2em] mb-2 ml-1">Output File Name</label>
                <input type="text" id="batchName" 
                       placeholder="e.g. Workshop_Attendance_2026" 
                       class="w-full px-5 py-4 bg-slate-50 border border-slate-200 rounded-2xl focus:ring-4 focus:ring-green-100 focus:border-green-500 focus:bg-white outline-none transition-all placeholder-slate-300 text-slate-700 font-semibold">
            </div>

            <div id="dropZone" class="group border-2 border-dashed border-slate-200 rounded-3xl p-12 flex flex-col items-center justify-center bg-slate-50 hover:bg-slate-100 hover:border-slate-300 transition-all cursor-pointer relative overflow-hidden">
                <div id="iconContainer" class="bg-white p-4 rounded-2xl shadow-sm group-hover:scale-110 transition-transform duration-300">
                    <svg id="uploadIcon" class="w-10 h-10 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path>
                    </svg>
                </div>
                <p id="mainText" class="text-base text-slate-600 font-bold mt-5">Select Member CSV</p>
                <p id="subText" class="text-xs text-slate-400 mt-1">DataListing-AACMemberDetails.csv</p>
                <input type="file" id="fileInput" accept=".csv" class="hidden">
            </div>

            <button id="processBtn" disabled class="w-full bg-slate-400 text-white font-black py-5 rounded-2xl transition-all shadow-lg shadow-slate-200 opacity-50 cursor-not-allowed flex items-center justify-center gap-2 tracking-wide text-lg">
                <span>Transform & Download</span>
            </button>

            <div id="status" class="text-center text-xs font-bold uppercase tracking-widest hidden py-3 rounded-xl"></div>
        </div>

        <footer class="mt-12 pt-8 border-t border-slate-50 text-center">
            <div class="flex items-center justify-center gap-2 mb-2">
                <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Local Privacy Mode Active</p>
            </div>
        </footer>
    </div>

    <script>
        const fileInput = document.getElementById('fileInput');
        const dropZone = document.getElementById('dropZone');
        const processBtn = document.getElementById('processBtn');
        const status = document.getElementById('status');
        const iconContainer = document.getElementById('iconContainer');
        const uploadIcon = document.getElementById('uploadIcon');
        const mainText = document.getElementById('mainText');
        const subText = document.getElementById('subText');

        dropZone.onclick = () => fileInput.click();

        fileInput.onchange = (e) => {
            if (e.target.files.length > 0) {
                const file = e.target.files[0];
                
                // UI Transformation to Green State
                dropZone.classList.replace('bg-slate-50', 'bg-green-50');
                dropZone.classList.replace('border-slate-200', 'border-green-400');
                iconContainer.classList.add('bg-green-500');
                uploadIcon.innerHTML = '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>';
                uploadIcon.classList.replace('text-slate-400', 'text-white');
                mainText.innerText = "File Ready";
                mainText.classList.replace('text-slate-600', 'text-green-700');
                subText.innerText = file.name;
                subText.classList.replace('text-slate-400', 'text-green-600');

                // Enable Button
                processBtn.disabled = false;
                processBtn.classList.remove('bg-slate-400', 'opacity-50', 'cursor-not-allowed');
                processBtn.classList.add('bg-green-600', 'hover:bg-green-700', 'shadow-green-100', 'active:scale-95');
                
                status.innerText = "Ready to transform";
                status.className = "text-center text-xs font-bold uppercase tracking-widest text-green-600 block";
            }
        };

        processBtn.onclick = () => {
            const file = fileInput.files[0];
            if (!file) return;

            Papa.parse(file, {
                header: true,
                skipEmptyLines: true,
                complete: function(results) {
                    const transformedData = results.data.map(row => {
                        // 1. Transform NricAddress_UnitNo (Floor before -, UnitNo after -)
                        let floor = "";
                        let unitNo = "";
                        const unitRaw = row.NricAddress_UnitNo || "";
                        if (unitRaw.includes('-')) {
                            const [f, u] = unitRaw.split('-');
                            floor = f.trim();
                            unitNo = u.trim();
                        }

                        // 2. Transform FullAddress (StreetName between 1st and 2nd comma)
                        let streetName = "";
                        const fullAddr = row.FullAddress || "";
                        const addrParts = fullAddr.split(',');
                        if (addrParts.length > 1) {
                            streetName = addrParts[1].trim();
                        }

                        return { ...row, Floor: floor, UnitNo: unitNo, StreetName: streetName };
                    });

                    downloadCSV(transformedData);
                }
            });
        };

        function downloadCSV(data) {
            const csv = Papa.unparse(data);
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement("a");
            
            const batchName = document.getElementById('batchName').value.trim().replace(/\s+/g, '_') || 'transformed_data';
            link.setAttribute("href", url);
            link.setAttribute("download", `${batchName}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            status.innerText = "Download Complete!";
            status.className = "text-center text-xs font-bold uppercase tracking-widest text-blue-600 block";
        }
    </script>
</body>
</html>
Key Functional Updates:
Initial State: The "Transform & Download" button is greyed out, disabled, and the cursor changes to a "not-allowed" icon when hovering.

Upload Feedback: Once you select the CSV, the entire dashed area turns green, the icon changes to a checkmark, and the text turns green to confirm the file is loaded.

Button Activation: The button instantly changes from grey to a vibrant green and becomes clickable.

Local Download: When the button is clicked, it applies your specific logic (Floor/Unit split and Street Name extraction) and triggers a local download.

create readme. output in markdown

Your Markdown README file is ready. It includes deployment instructions for GitHub Pages, an explanation of the transformation logic for Floor, UnitNo, and StreetName, and details on the privacy-first local processing.
MD icon
 README 
MD





Gemini is AI and can make mistakes.

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
