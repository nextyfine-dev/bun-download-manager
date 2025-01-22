# 🛠️ bun-downloader-manager

`bun-downloader-manager` is a simple yet powerful package manager-like download manager built with **Bun.js**. It allows you to download files sequentially or with a queue-based approach, handling retries, concurrency limits, and custom tasks efficiently. Designed for seamless integration into Node.js and TypeScript projects, this tool is perfect for managing downloads with ease and flexibility.

---

## 📚 Table of Contents

- [�️ bun-downloader-manager](#️-bun-downloader-manager)
  - [📚 Table of Contents](#-table-of-contents)
  - [🔧 Features](#-features)
  - [🚀 Installation](#-installation)
    - [Using npm](#using-npm)
    - [Using yarn](#using-yarn)
    - [Using bun](#using-bun)
  - [🔍 Usage](#-usage)
    - [Queue Download Example](#queue-download-example)
    - [Queue Download Example 2](#queue-download-example-2)
    - [Simple Download Example](#simple-download-example)
    - [Custom Task Example Using `otherTaskFunction`](#custom-task-example-using-othertaskfunction)
  - [🛠️ API Documentation](#️-api-documentation)
    - [DownloadManager Class](#downloadmanager-class)
      - [Constructor Options](#constructor-options)
      - [Methods](#methods)
    - [Queue Class](#queue-class)
      - [Constructor Options](#constructor-options-1)
      - [Methods](#methods-1)
  - [⚖️ License](#️-license)

---

## 🔧 Features

- **Queue-based downloads**: Handles multiple download tasks with retry mechanisms and concurrency limits.
- **Simple sequential downloads**: Downloads files one by one.
- **Retry support**: Automatically retries failed downloads up to a specified limit.
- **Custom file naming**: Allows you to define custom logic for naming downloaded files.
- **Logging**: Optional logging to monitor download progress.
- **Custom tasks**: Supports additional processing for each file with `otherTaskFunction`.
- **File overwrite control**: Enables or disables overwriting files with the same name.

---

## 🚀 Installation

To install `bun-downloader-manager`, you can use **npm**, **yarn**, or **bun**:

### Using npm

```bash
npm install bun-downloader-manager
```

### Using yarn

```bash
yarn add bun-downloader-manager
```

### Using bun

```bash
bun add bun-downloader-manager
```

---

## 🔍 Usage

Here are some examples of how to use `bun-downloader-manager` in your project:

### Queue Download Example

Download multiple files with concurrency control and automatic task management:

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
  "https://example.com/file3.jpeg",
];

const downloadManager = new DownloadManager({
  consoleLog: true, // Enable logging
  method: "queue", // Use queue-based download
});

await downloadManager.download(urls);
```

### Queue Download Example 2

Manually enqueue download tasks for greater flexibility:

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
  "https://example.com/file3.jpeg",
];

// Optional custom file name logic
const getFileName = (url: string) => `${Date.now()}-${url.split("/").pop()}`;

const downloadManager = new DownloadManager({
  consoleLog: true, // Enable logging
});

for (const url of urls) {
  downloadManager.enqueueDownloadTask(url, getFileName(url)); // Custom file name is optional
}
```

### Simple Download Example

Download files one by one in sequence:

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
];

const downloadManager = new DownloadManager({
  consoleLog: true,
  method: "simple", // Use simple sequential download
});

await downloadManager.download(urls);
```

### Custom Task Example Using `otherTaskFunction`

Define a custom task function for additional processing during downloads:

```ts
import DownloadManager from "bun-downloader-manager";

const urls = [
  "https://example.com/file1.jpeg",
  "https://example.com/file2.jpeg",
];

const customTaskFunction = async (url: string, fileName: string) => {
  console.log(`Processing ${fileName}`);
  // Custom logic (e.g., saving metadata or sending notifications)
};

const downloadManager = new DownloadManager({
  method: "queue",
  consoleLog: true,
  otherTaskFunction: customTaskFunction, // Custom task
  overWriteFile: true, // Overwrite existing files
});

await downloadManager.download(urls);
```

---

## 🛠️ API Documentation

### DownloadManager Class

The `DownloadManager` class is the main interface for handling file downloads.

#### Constructor Options

| Option              | Type                                               | Default         | Description                                                                                  |
| ------------------- | -------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------- |
| `method`            | `"simple"` \| `"queue"`                            | `"queue"`       | Choose between **simple** (sequential downloads) or **queue** (task-based downloads).        |
| `concurrencyLimit`  | `number`                                           | `5`             | Maximum number of concurrent downloads (for "queue" method).                                 |
| `retries`           | `number`                                           | `3`             | Maximum number of retries allowed for failed downloads.                                      |
| `consoleLog`        | `boolean`                                          | `false`         | Enable or disable console logging.                                                           |
| `downloadFolder`    | `string`                                           | `"./downloads"` | Directory to save downloaded files.                                                          |
| `getFileName`       | `(url: string) => string`                          | `undefined`     | Custom function to generate file names from URLs. If not provided, the URL filename is used. |
| `otherTaskFunction` | `(url: string, fileName: string) => Promise<void>` | `undefined`     | Custom task function for additional processing (e.g., post-download actions).                |
| `overWriteFile`     | `boolean`                                          | `false`         | If `true`, files with the same name will be overwritten. If `false`, duplicates are skipped. |

#### Methods

- **`download(urls: string | string[])`**: Downloads one or more files based on the configured `method`.
- **`enqueueDownloadTask(url: string, fileName?: string, priority?: number)`**: Adds a download task to the queue manually.

### Queue Class

Manages tasks in the queue with concurrency control.

#### Constructor Options

| Option             | Type      | Default | Description                          |
| ------------------ | --------- | ------- | ------------------------------------ |
| `concurrencyLimit` | `number`  | `5`     | Maximum number of concurrent tasks.  |
| `maxRetries`       | `number`  | `3`     | Maximum retries for failed tasks.    |
| `consoleLog`       | `boolean` | `false` | Enable logging for queue operations. |

#### Methods

- **`enqueue(task: Task)`**: Adds a new task to the queue.
- **`runNext()`**: Processes the next task in the queue.

---

## ⚖️ License

This project is licensed under the **MIT License**. See the [LICENSE](./LICENSE) file for details.
