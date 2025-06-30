

# 🧭 OCTRA GUIDE

A step-by-step walkthrough for setting up your OCTRA wallet, configuring the pre-client, and interacting with the testnet — even if you're not a technical user.

---

## ✅ STEP 1: WALLET SETUP

### 🔧 Prerequisites

* Git

> 💡 If Git is not installed:

* Visit this link to install Git for your operating system:
  👉 [https://github.com/git-guides/install-git]
---

### 🛠 Instructions

1. **Open a terminal or command prompt**

2. **Check if Git is installed**

   ```bash
   git --version
   ```

   * If Git is installed correctly, it will return the version number (e.g., `git version 2.41.0`).

3. **Clone the wallet generator repository**

   ```bash
   git clone https://github.com/octra-labs/wallet-gen.git
   ```

4. **Change into the cloned folder**

   ```bash
   cd wallet-gen
   ```

5. **Run the wallet generator**

   * On **Linux and macOS**:

     ```bash
     chmod +x ./start.sh
     ./start.sh
     ```

   * On **Windows**:

     ```cmd
     start.bat
     ```

6. **Follow the prompts in the terminal**

7. **Open your browser and go to this URL**

   ```
   http://localhost:8888
   ```

8. **Backup your wallet information**

   * Make sure to save the details shown on the screen and the text file generated.
   * You will need these in the next step.

---

## ✅ STEP 2: OCTRA PRE-CLIENT SETUP

### 🔧 Prerequisites

* Python

> 💡 To install Python:

* On Windows: [https://www.python.org/downloads/]
* On Linux and macOS, run:

  ```bash
  sudo apt install python3
  ```

---

### 🛠 Instructions

1. **Open a terminal or command prompt**

2. **Check if Python is installed**

   ```bash
   python3 --version
   ```

3. **Clone the pre-client repository**

   ```bash
   git clone https://github.com/octra-labs/octra_pre_client.git
   ```

4. **Change into the repo directory**

   ```bash
   cd octra_pre_client
   ```

5. **Create a virtual environment**

   ```bash
   python3 -m venv venv
   ```

6. **Activate the virtual environment**

   * On **Linux and macOS**:

     ```bash
     source venv/bin/activate
     ```
   * On **Windows**:

     ```cmd
     venv\Scripts\activate
     ```

7. **Install required dependencies**

   ```bash
   pip install -r requirements.txt
   ```

8. **Copy the example wallet configuration**

   ```bash
   cp wallet.json.example wallet.json
   ```

9. **Open the wallet.json file in your text editor**

   The contents should look like this:

   ```json
   {
     "priv": "private-key-here",
     "addr": "octxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
     "rpc": "https://octra.network"
   }
   ```

10. **Replace the placeholders with your real data:**

    * Replace `"private-key-here"` with your **private key**
    * Replace `"octxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"` with your **wallet address**

> 📂 You can find these details in the file created during wallet generation or return to your browser if the generator is still running.

11. **Run the CLI**

    * On **Linux/macOS**:

      ```bash
      ./run.sh
      ```
    * On **Windows**:

      ```cmd
      run.bat
      ```

---

## ✅ STEP 3: Interacting with the Testnet & Sending Multiple Transactions

### 🔧 Prerequisites

* Visual Studio Code (VSCode)
* Rainbow CSV extension for VSCode

---

### 💡 Getting Wallet Addresses

Generating many wallets manually is difficult. Here are two options:

---

### ✅ Option 1: Use Pre-Fetched Wallets

Use the list of addresses available here:
👉 [https://github.com/0xUnavailable/fetch/blob/master/addresses.csv]


---

### ✅ Option 2: Scrape Wallet Addresses Yourself

1. **Open a terminal**

2. **Clone the fetch repository**

   ```bash
   git clone https://github.com/0xUnavailable/fetch.git
   ```

3. **Change into the folder**

   ```bash
   cd fetch
   ```

4. **Install dependencies**

   ```bash
   npm install axios cheerio
   ```

> 📘 You can also read the README file here for more information:
> 👉 [https://github.com/0xUnavailable/fetch/blob/master/README.md]


5. **Run the scraper**

   ```bash
   node scrape https://octrascan.io/staging --text-only a
   ```

   * This scrapes the staging website and returns all values inside anchor tags (`<a>`)
   * The scraped data is saved in a `.csv` file in the folder

---

### 🎨 Formatting the Scraped CSV

1. **Open the scraped `.csv` file in VSCode**

2. **Install Rainbow CSV extension if you haven't already**

3. **Right-click in the CSV file → Rainbow CSV → RBQL Query**

4. **Run the following query**

   ```sql
   SELECT DISTINCT a3 where like(a3,'oct%')
   ```

   * This removes duplicates and keeps only values starting with `oct`

---

### ➕ Append Amount to Each Address

1. Press `Ctrl + H` to open **Find and Replace**

2. Enable **Regex** by clicking the `.*` button

3. In the **Find** field, enter:

   ```
   ^(oct\w+)$
   ```

4. In the **Replace** field, enter:

   ```
   $1 0.135
   ```

5. Click **Replace All**

> 🎯 This appends `0.135` to each address.
> Example:

```
Before: oct3d9toQriuYdiZgjqXHKEE2wEVakEE1DKBxUTozRqtmkE  
After:  oct3d9toQriuYdiZgjqXHKEE2wEVakEE1DKBxUTozRqtmkE 0.135
```

---

### 🔁 Change the Value If Needed

1. Press `Ctrl + H` again

2. In the **Find** field, enter:

   ```
   (\boct\w+\b) 0\.135
   ```

3. In the **Replace** field, enter:

   ```
   $1\n
   ```

4. Click **Replace All**

> 🔄 This removes the appended value and places each address on its own line again — useful if you want to add a different amount.

---

## 🎉 You're Done!

