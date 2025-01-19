---
title: "Learn how to use the Kaggle API"
author: "Karen Sánchez"
date: "1/19/2025"
categories: [Kaggle, API, Dataset,TIL]
---

Today I've learned how to download datasets from Kaggle using its API. Previously, I used to download datasets manually, but with the API, the process is much faster and more efficient.

Here’s a step-by-step guide to downloading datasets using the Kaggle API, inspired by fast.ai's [Live Coding 7 session](https://youtu.be/cagqUrHMDJ0?list=PLfYUBJiXbdtSLBPJ1GMx-sQWf6iNhb8mM&t=617).

## Step 1: Verify Kaggle Installation
First, check if Kaggle is installed by running the following command in your terminal:
```bash
kaggle
```

## Step 2: Install the Kaggle Package
If the `kaggle` command is not recognized, install the package using pip:
```bash
pip install --user kaggle
```
You might see a warning similar to:
```
WARNING: The script slugify is installed in '/home/user/.local/bin' which is not on PATH.
```

Kaggle command line tool has been installed in your home directory (because of the `--user` flag in the previous command), specifically in the `.local/bin` folder, where there are executable files. However, this folder is not included in the `PATH` environment variable, the place where the shell looks for executables files associated with a given command. (You can watch [the part of Live Coding 3](https://youtu.be/B6BQiIgiEks?list=PLfYUBJiXbdtSLBPJ1GMx-sQWf6iNhb8mM&t=564), where Jeremy Howard talks about the concept `PATH` in the shells.)  As a result, typing `kaggle` again still doesn't work out. We'll address this issue in step number 3.

## Step 3: Update the PATH Variable
To resolve this warning, modify your PATH variable as follows:

1. Open your `.bashrc` file using vim:
   ```bash
   vim ~/.bashrc
   ```
2. Navigate to the end of the file with `Shift+g`.
3. Open a new line by pressing `o`, and it automatically places the cursor in "Insert Mode" (you should see -- INSERT -- at the bottom of the screen).
4. Add the following line: 
   ```bash
   export PATH=~/.local/bin:$PATH
   ```
   You might want to copy the above line to your clipboard (usually with `Ctrl+c`) and paste it in vim with a right-click of your mouse (but it depends on your terminal emulator).
5. Press `Esc` to exit "Insert Mode".
6. Save and exit vim with `:wq`, then press `Enter`.
7. Update your shell with the new PATH by running:
   ```bash
   source !$
   ```
   `source` is used to read and execute the contents of a file in the current shell session, and `!$` represents the argument of the last command (remember it was `vim ~/.bashrc`), so in this case `!$` is the same as `~/.bashrc`.

Now, the `kaggle` command should be recognized.

## Step 4: Add Your Kaggle API Token
The Kaggle API requires a `kaggle.json` file containing your credentials. Follow these steps to obtain and configure the file:

1. Visit Kaggle, click on your profile picture and select **Settings**.
2. Scroll to the **API** section and click **Create New Token**.
3. A `kaggle.json` file will be downloaded to your system.

For Windows users, you can locate the file in your Downloads folder, typically at `/mnt/c/Users/<YourUsername>/Downloads`

4. Create a `.kaggle` directory in your home folder:
   ```bash
   mkdir ~/.kaggle
   ```
5. Copy the `kaggle.json` file to this directory:
   ```bash
   cp /mnt/c/Users/<YourUsername>/Downloads/kaggle.json ~/.kaggle
   ```
6. Secure your API token by modifying its permissions:
   ```bash
   chmod 600 ~/.kaggle/kaggle.json
   ```

## Step 5: Download a Dataset
To download a dataset, navigate to its Kaggle page and copy the download command. For example, to download the dataset for the [Playground Series Season 5 Episode 1 competition](https://www.kaggle.com/competitions/playground-series-s5e1/data):

1. Accept the competition rules on Kaggle, if applicable.
2. Navigate to the directory in the terminal where you want to save the data.
3. Run the download command:
   ```bash
   kaggle competitions download -c playground-series-s5e1
   ```

## Step 6: Extract the Dataset
The downloaded file will be a ZIP archive. Extract it using:
```bash
unzip -q playground-series-s5e1.zip
```

## Conclusion
And that’s it! From now on, with the Kaggle API set up, you can download datasets effortlessly using just two commands:
```bash
kaggle competitions download -c <dataset-name>
unzip -q <dataset-name>.zip
```