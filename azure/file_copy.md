# Azure File Copy

Make sure you have the command line tools for Azure and setup your subscription

```bash
az account set --subscription <subscription>
```

### Single file remote to local
Copy a single file from remote file share to local

```bash
az storage file download --account-name <storage_account_name> -s <storage_share_name> -p <path_to_remote_file> --dest <local_file>
```

### Single file upload from local to remote
```bash
az storage file upload --account-name <storage_account_name> -s <storage_share_name> -p <path_to_remote_file> --source <local_file>
```

### Batch upload multiple files local to remote

```bash
az storage file upload-batch --account-name <storage_account_name> -d <remote_path> --source 
<local_path>
```
 




