# Charge-vue: Vue.js Transaction Tracker

## Overview

Charge-vue is a dynamic, user-friendly frontend application developed with Vue.js, designed for efficient tracking and management of your financial transactions. Whether you're looking to maintain a personal budget, manage business expenses, or just keep track of your spending habits, Charge-vue provides a comprehensive solution to monitor your financial health in real-time.

## Key Features

<ul>
    <li><b>Email Notifications: </b>If the total <u>balance ever becomes negative</u>, the application utilizes Nodemailer to <u>send an email notification to a specific email address</u>. To ensure reliability and manageability, this email sending process is queued in BullMQ before being processed.</li>
    <li><b>Transaction Counter/History:</b> Easily view your current financial status, with clear indicators for positive and negative balances.</li>
    <li><b>Add, Delete, and Edit Transactions:</b> Full control over your transaction records, allowing for seamless management of your financial data.</li>
    <li><b>Real-Time Financial Health Alerts:</b> Automatically receive email notifications if your balance dips into the negatives, helping you avoid potential financial pitfalls.</li>
    <li><b>Intuitive User Interface:</b> A clean, straightforward design ensures you can navigate and manage your transactions with ease.</li>
</ul>

## Getting Started

### Clone the Repository

Begin by cloning the frontend repository to your local environment:

```bash
git clone https://github.com/BertoPolo/charge-vue.git
```

### Install Dependencies

After cloning, navigate into the project directory and install the necessary dependencies:

```bash
cd charge-vue
npm install

```

### Running the Application

To launch charge-vue in development mode with hot-reload functionality, use the following command:

```bash
npm run dev
```

Open http://localhost:5173/
