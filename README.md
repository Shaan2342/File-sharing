 a powerful file-sharing application designed for secure and efficient sharing of encrypted data. It offers both a web application and a command-line interface (CLI) for versatile use cases. The application is built with a focus on end-to-end encryption, providing users with the ability to encrypt and decrypt files directly in the browser.

Features
On-the-Fly Encryption and Decryption: Experience seamless encryption and decryption processes directly in your browser. For example, decrypt movies without downloading them first.

Multi-Files Upload: Efficiently upload multiple files with on-the-fly creation of Zip archives, all handled within the web application without temporary archive creation.

Pause & Resume Uploads: Enjoy the flexibility to pause and resume file uploads at your convenience.

Automatic Upload Resuming: Web application users benefit from automatic resuming of uploads in case of connection failures or timeouts.

Lightweight Web Application: The web application is designed to be lightweight, with HTML/CSS/JS totaling less than 100kb.

File Size Limitation & Timeout: Traditional features like file size limitation and timeouts are also supported for a well-rounded file-sharing experience.

Server Installation & Configuration
Quick'n'dirty
To quickly try secsend, run a server directly from your shell:

bash
Copy code
$ pip install secsend_api secsend_webapp
$ sanic secsend_api.prod.app -p 8000
Access secsend by navigating to http://127.0.0.1:8000. If secsend_webapp is not installed, only the command-line interface will be available. Uploaded files are stored in the secsend_root directory by default.

Run with Docker
Copy docker.env.example to docker.env and modify its content to configure secsend (e.g., file size limit).
Run secsend with Docker:
bash
Copy code
# docker run --env-file docker.env -p 8000:80 -v /path/to/data/storage:/data aguinet/secsend:v1.0.0
/path/to/data/storage will store uploaded files and associated metadata.

Run with systemd
For running secsend on a server using systemd:

Create a Python virtualenv and install secsend:
bash
Copy code
$ virtualenv secsend_venv && . secsend_venv/bin/activate
$ pip install secsend_api secsend_webapp
Declare the secsend service in systemd by creating /etc/systemd/system/secsend.service with appropriate content. Example:
ini
Copy code
[Unit]
Description=secsend

[Service]
ExecStart=/path/to/secsend_venv/bin/sanic secsend_api.prod.app -p 8000 -H 127.0.0.1
Environment=PYTHONUNBUFFERED=1
Environment=SECSEND_BACKEND_FILES_ROOT=/path/to/data/storage
Restart=always
User=www-send

[Install]
WantedBy=multi-user.target
Replace /path/to/data/storage with a writable directory. See the configuration section for more environment variables.

Enable & run the secsend service:
bash
Copy code
$ systemctl enable --now secsend.service
secsend is now accessible at http://127.0.0.1:8000.

Configuration
Configure secsend using environment variables:

SECSEND_FILESIZE_LIMIT: Maximum file size in bytes (0 means no limit).
SECSEND_TIMEOUT_S_VALID: Valid time limits, as a comma-separated list of seconds (0 seconds means no limit).
SECSEND_BACKEND_FILES_ROOT: Path to secsend's data storage.
Command Line Usage
Installation
bash
Copy code
$ pip install secsend
Upload a File
bash
Copy code
$ secupload myvideo.mp4 https://send.domain.com
secupload generates an administration link for resuming or deleting the file and a download link for recipients.

Resume an upload using an administration link:

bash
Copy code
$ secupload -c myvideo.mp4 https://send.domain.com/dl?id=XXXXXX#YYYYY
Download a File
bash
Copy code
$ secdownload https://send.domain.com/dl?id=XXXXXX#YYYYY
By default, the original filename will be used as the destination filename. Use -o to override this.

Delete an Uploaded File
bash
Copy code
$ secadmin -d https://send.domain.com/dl?id=XXXXXX#YYYYY
Use an administration link for this to work.




