# 🛠️ bun-downloader-manager

`bun-downloader-manager` is a simple yet powerful package manager-like download manager built with **Bun.js**. It allows you to download files sequentially or with a queue-based approach, handling retries and concurrency limits efficiently. This tool is designed to integrate seamlessly into Node.js applications, providing an easy-to-use interface for managing downloads.

## 📚 Table of Contents

- [🛠️ bun-downloader-manager](#️-bun-downloader-manager)
  - [📚 Table of Contents](#-table-of-contents)
  - [🔧 Features](#-features)
  - [🚀 Installation](#-installation)
    - [Using npm:](#using-npm)
    - [Using yarn:](#using-yarn)
    - [Using bun:](#using-bun)
  - [🔍 Usage](#-usage)
    - [Queue Download Example](#queue-download-example)
    - [Queue Download Example 2](#queue-download-example-2)
    - [Simple Download Example](#simple-download-example)
    - [Using Custom File Name Logic](#using-custom-file-name-logic)
  - [🛠️ API Documentation](#️-api-documentation)
    - [DownloadManager Class](#downloadmanager-class)
      - [Constructor Options:](#constructor-options)
      - [Methods:](#methods)
    - [Queue Class](#queue-class)
      - [Constructor Options:](#constructor-options-1)
      - [Methods:](#methods-1)
  - [⚖️ License](#️-license)

## 🔧 Features

- **Queue-based downloads**: Handles multiple download tasks with retry mechanisms and concurrency limits.
- **Simple sequential downloads**: Allows simple file downloads one by one.
- **Retry support**: Automatically retries failed downloads up to a specified limit.
- **Custom file naming**: Lets you define custom naming logic for downloaded files.
- **Logging**: Optional logging to monitor the progress and status of downloads.

## 🚀 Installation

To install `bun-downloader-manager`, you can use **npm** or **yarn**:

### Using npm:

```bash
npm install bun-downloader-manager
```

### Using yarn:

```bash
yarn add bun-downloader-manager
```

### Using bun:

```bash
bun add bun-downloader-manager
```

## 🔍 Usage

Here are some examples of how you can use `bun-downloader-manager` in your project.

### Queue Download Example

The **queue method** downloads multiple files with concurrency control. It uses a priority-based queue to manage download tasks.

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
  "https://example.com/file3.jpeg",
];

const getFileName = (url: string) => {
  return `${Date.now()}-${url.split("/").pop()}`;
};

// Queue download with automatic task management
const downloadManager = new DownloadManager({
  consoleLog: true, // Enable logging
  method: "queue", // Use queue-based download
});

await downloadManager.download(urls);
```

In this example, the downloads will be handled in a queue with a maximum concurrency limit of 5 (default).

### Queue Download Example 2

In this example, tasks are added to the queue manually using `enqueueDownloadTask`. This gives you more flexibility in managing each download task individually.

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
  "https://example.com/file3.jpeg",
];

// Optional custom file name logic (you can skip providing this if you don't need it)
const getFileName = (url: string) => {
  return `${Date.now()}-${url.split("/").pop()}`;
};

// Queue download example 2: add tasks manually
const downloadManager2 = new DownloadManager({
  consoleLog: true, // Enable logging
});

for (const url of urls) {
  // You can provide a custom file name logic (optional)
  downloadManager2.enqueueDownloadTask(url, getFileName(url)); // Custom file name logic is optional
}
```

In this example, each URL is manually added to the queue using the `enqueueDownloadTask` method. The `getFileName` function is optional. If you don't provide it, a default file name will be used based on the URL.

### Simple Download Example

The **simple method** downloads files one by one in sequence, without managing a queue.

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
  "https://example.com/file3.jpeg",
];

// Simple download example: download files sequentially
const downloadManager = new DownloadManager({
  consoleLog: true,
  method: "simple", // Use simple sequential download
});

await downloadManager.download(urls);
```

This method is useful when you don't need concurrency and want to download files one by one.

### Using Custom File Name Logic

You can define your own logic for generating file names during the download process.

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
];

// Define custom file name logic
const getFileName = (url: string) => {
  let fileName = url.split("/").pop();
  return `downloaded-${Date.now()}-${fileName}`;
};

// Download manager with custom file name logic
const downloadManager = new DownloadManager({
  consoleLog: true,
  getFileName, // Custom logic for file name
});

await downloadManager.download(urls);
```

In this example, files will be saved with a name like `downloaded-1645184557778-file1.jpeg`.

## 🛠️ API Documentation

### DownloadManager Class

The `DownloadManager` class is the main interface for handling file downloads. You can configure it with various options:

#### Constructor Options:

- `method` (optional): Choose between `"simple"` or `"queue"` method for download management.
- `concurrencyLimit` (optional): The maximum number of concurrent downloads (default is `5`).
- `retries` (optional): The maximum number of retries allowed for failed downloads (default is `3`).
- `consoleLog` (optional): Enable console logging (default is `false`).
- `downloadFolder` (optional): Directory to save the downloaded files (default is `./downloads`).
- `getFileName` (optional): A custom function to generate file names from URLs.

#### Methods:

- `download(urls: string | string[])`: Handles multiple URL downloads based on the configured method (either `"simple"` or `"queue"`).
- `enqueueDownloadTask(url: string, fileName: string, priority: number)`: Adds a download task to the queue with a specific priority.

### Queue Class

The `Queue` class manages download tasks, ensuring that they are processed with a controlled concurrency.

#### Constructor Options:

- `concurrencyLimit`: The maximum number of tasks to run concurrently.
- `maxRetries`: The maximum number of retries for failed tasks.
- `consoleLog`: Enable logging for the queue.

#### Methods:

- `enqueue(task: Task)`: Adds a new task to the queue.
- `runNext()`: Processes the next available task in the queue.

## ⚖️ License

This project is licensed under the **MIT License**. See the [LICENSE](./LICENSE) file for more information.
