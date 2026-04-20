# DS_Projects — Complete Setup Guide (Step-by-Step)

Every single step you need to follow, from creating the GitHub repo to being ready for Task 1 work.

---

## Phase 1: Create the GitHub Repository (Browser)

1. Open your browser and go to **https://github.com/new**
2. Fill in the form:
   - **Repository name:** `DS_Projects`
   - **Description:** `Data Science Take-Home Projects — Funnel Analysis, A/B Testing, Revenue Prediction`
   - **Visibility:** ✅ Public
   - ⚠️ **Do NOT** check "Add a README file"
   - ⚠️ **Do NOT** add `.gitignore` or license — we'll create them locally
3. Click **"Create repository"**
4. You'll see a page with setup instructions — **keep this tab open**, you'll need the repo URL later (it will look like `https://github.com/YOUR_USERNAME/DS_Projects.git`)

---

## Phase 2: Create the Local Project Folder (Git Bash)

Open **Git Bash** and run these commands one by one:

```bash
# Step 1: Go to Desktop
cd /c/Users/Home/Desktop

# Step 2: Create the main project folder
mkdir DS_Projects

# Step 3: Enter it
cd DS_Projects

# Step 4: Initialize Git
git init
```

---

## Phase 3: Create the Virtual Environment (Git Bash)

Still in the same Git Bash terminal, inside `DS_Projects/`:

```bash
# Step 5: Create the virtual environment
python -m venv venv

# Step 6: Activate it
source venv/Scripts/activate
```

> You should now see `(venv)` at the beginning of your prompt. This means it's active.

```bash
# Step 7: Upgrade pip
python -m pip install --upgrade pip

# Step 8: Install all dependencies for all 3 tasks
pip install pandas numpy matplotlib seaborn plotly scipy statsmodels scikit-learn xgboost lightgbm jupyter ipykernel

# Step 9: Register the Jupyter kernel (so your IDE can use it)
python -m ipykernel install --user --name=ds_projects --display-name "Python (DS Projects)"
```

---

## Phase 4: Create the `.gitignore` File (Git Bash)

Still in `DS_Projects/`:

```bash
# Step 10: Create the .gitignore file
cat > .gitignore << 'EOF'
# ========================
# DS_Projects .gitignore
# ========================

# Virtual environments
venv/
.venv/
env/

# Jupyter checkpoints
.ipynb_checkpoints/

# Python cache
__pycache__/
*.pyc
*.pyo

# OS junk
__MACOSX/
.DS_Store
Thumbs.db
desktop.ini

# Task description files (private)
*.docx

# IDE settings
.vscode/
.idea/

# Environment variables
.env
EOF
```

---

## Phase 5: Create the Project Folder Structure (Git Bash)

```bash
# Step 11: Create the 3 project folders
mkdir task_1_subscription_funnel
mkdir task_2_ab_test_analysis
mkdir task_3_revenue_prediction
```

---

## Phase 6: Copy Data Files Into the Correct Folders (Git Bash)

Each task requires its data files to sit **right next to** the notebook (same directory), as stated in the task instructions.

```bash
# Step 12: Copy Task 1 data
cp "/c/Users/Home/Desktop/DS_Task1/task_1_dataset/Growth Task Data/app_open_data.csv" task_1_subscription_funnel/
cp "/c/Users/Home/Desktop/DS_Task1/task_1_dataset/Growth Task Data/event_data.csv" task_1_subscription_funnel/

# Step 13: Copy Task 2 data
cp "/c/Users/Home/Desktop/DS_Task1/task_2_dataset/ab_dataset.csv" task_2_ab_test_analysis/

# Step 14: Copy Task 3 data
cp "/c/Users/Home/Desktop/DS_Task1/task_3_dataset/task_dataset/train.csv" task_3_revenue_prediction/
cp "/c/Users/Home/Desktop/DS_Task1/task_3_dataset/task_dataset/test.csv" task_3_revenue_prediction/
```

---

## Phase 7: Create Empty Placeholder Files (Git Bash)

We'll create the notebook files and README files now (empty / placeholder), and fill them in as we work.

```bash
# Step 15: Create empty README files (we'll fill these at the very end)
echo "# DS Projects" > README.md
echo "# Task 1: Subscription Funnel Analysis" > task_1_subscription_funnel/README.md
echo "# Task 2: A/B Test Analysis" > task_2_ab_test_analysis/README.md
echo "# Task 3: 30-Day Revenue Prediction" > task_3_revenue_prediction/README.md
```

> [!NOTE]
> We do NOT create the `.ipynb` notebooks via Git Bash. We'll create them from the IDE (next phase) so they have the correct JSON structure and kernel attached.

---

## Phase 8: Generate `requirements.txt` (Git Bash)

```bash
# Step 16: Generate the pinned requirements file
pip freeze > requirements.txt
```

---

## Phase 9: Verify the Structure (Git Bash)

Run this to confirm everything looks correct:

```bash
# Step 17: Check the structure (ignoring venv)
find . -not -path './venv/*' -not -path './.git/*' | sort
```

You should see something like this:
```
.
./.gitignore
./README.md
./requirements.txt
./task_1_subscription_funnel
./task_1_subscription_funnel/README.md
./task_1_subscription_funnel/app_open_data.csv
./task_1_subscription_funnel/event_data.csv
./task_2_ab_test_analysis
./task_2_ab_test_analysis/README.md
./task_2_ab_test_analysis/ab_dataset.csv
./task_3_revenue_prediction
./task_3_revenue_prediction/README.md
./task_3_revenue_prediction/test.csv
./task_3_revenue_prediction/train.csv
```

> [!IMPORTANT]
> If anything is missing or in the wrong place, fix it before continuing.

---

## Phase 10: Create the Notebook for Task 1 (IDE)

1. Open the **`DS_Projects`** folder in your IDE (File → Open Folder → `C:\Users\Home\Desktop\DS_Projects`)
2. Inside `task_1_subscription_funnel/`, right-click → **New File**
3. Name it: `task_1_subscription_funnel.ipynb`
4. Open the notebook → click **Select Kernel** (top right)
5. Select **Python (DS Projects)** — the kernel we registered in Step 9
6. Type `print("Task 1 ready!")` in the first cell → press `Shift+Enter` to confirm it works
7. **Save** the file (`Ctrl+S`)

> Do the same later for Task 2 and Task 3 when you start working on them. For now, only Task 1 is needed.

---

## Phase 11: First Commit & Push to GitHub (Git Bash)

Go back to Git Bash (make sure you're in `DS_Projects/`):

```bash
# Step 18: Stage everything
git add .

# Step 19: Check what will be committed (sanity check)
git status
```

> Verify that `venv/`, `.ipynb_checkpoints/`, and `*.docx` files are **NOT** listed. Only your project files should appear.

```bash
# Step 20: First commit
git commit -m "chore: initialize DS_Projects repo structure with datasets"

# Step 21: Connect to GitHub (replace YOUR_USERNAME with your actual GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/DS_Projects.git

# Step 22: Set main branch and push
git branch -M main
git push -u origin main
```

> If prompted for credentials, enter your GitHub username and a **Personal Access Token** (not your password). If you've pushed before from this computer, it should work automatically.

---

## Phase 12: Verify on GitHub (Browser)

1. Go to `https://github.com/YOUR_USERNAME/DS_Projects`
2. Confirm you see:
   - `.gitignore`
   - `README.md`
   - `requirements.txt`
   - `task_1_subscription_funnel/` (with CSVs and README)
   - `task_2_ab_test_analysis/` (with CSV and README)
   - `task_3_revenue_prediction/` (with CSVs and README)
3. Confirm you do **NOT** see: `venv/`, `__MACOSX/`, `.docx` files, `.ipynb_checkpoints/`

---

## ✅ Done! You're Ready for Task 1

After completing all 12 phases above, your setup is complete. Come back here and tell me you're ready — we'll start working on the **Subscription Funnel Analysis** notebook together, making small commits as we go.

### Commit Strategy for Task 1 (how we'll work)
We'll commit after each major section:
```
feat(task-1): add data loading and initial exploration
feat(task-1): complete funnel construction and conversion rates
feat(task-1): add platform and country segmentation
feat(task-1): complete registration deep dive
feat(task-1): add behavioral analysis and session depth
feat(task-1): add recommendations and write-up
feat(task-1): finalize notebook with all outputs visible
```
