# OCI Block Volume Performance Testing (Terraform + Jupyter Notebook)

## Objective
Provision a temporary OCI compute instance and block volume with Terraform, then benchmark the block volume on the instance using `fio`. Results are exported to CSV with key test metadata.

## What the notebook does
- Loads OCI auth from `~/.oci/config` (no secrets in code).
- Presents an interactive UI to select Region, Compartment, AD, Shape (and OCPUs/Memory for Flex), a compatible Image, SSH keys, and the initial Block Volume size/VPUs.
- Writes and applies Terraform to create the test environment, then retrieves instance/volume outputs.
- Formats and mounts the volume once, grows the filesystem online for subsequent runs, and executes `fio`.
- Exports results to CSV with metadata: Region, Shape, OCPUs, Memory(GB), Block Size, IO Depth, Runtime(s), NumJobs, Size (GB), VPUs/GB, Write/Read throughput.

## Configure (Cell 2 only)
Edit variables in Cell 2 (single source of truth):
- OCI: `OCI_CONFIG_FILE`, `OCI_PROFILE`
- `fio`: `FIO_BLOCK_SIZE`, `FIO_IODEPTH`, `FIO_RUNTIME`, `FIO_NUMJOBS`
- Test sweep: `SIZES`, `VPUS`
- (Optional) SSH key paths if not using defaults

## Run
1. Cell 1 — install Python dependencies  
2. Cell 2 — set variables (edit here only)  
3. Cell 3 — make selections and click “Apply Selections”  
4. Cells 4 → 5 → 6 — write/apply Terraform  
5. Cell 7 — fetch Terraform outputs  
6. Cell 8 — run tests and export CSV  
7. Cell 9 — initialize providers and destroy resources

## Output
- CSV: `OCIBlockVolumeTesting/fio_results.csv`  
- Columns include: Region, Shape, OCPUs, Memory(GB), Block Size, IO Depth, Runtime(s), NumJobs, Size (GB), VPUs/GB, Write (MiB/s), Read (MiB/s), Write (MB/s), Read (MB/s)

## Cleanup
Run Cell 9 to destroy all Terraform-managed resources when finished.
