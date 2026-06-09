# Delete LDS Configurations

Jupyter notebook to delete all LDS (Log Delivery Service) configurations via the Akamai API. LDS is being decommissioned end of June 2026 — customers should migrate to Datastream.

## Prerequisites

- An Akamai `~/.edgerc` file with valid credentials
- [uv](https://docs.astral.sh/uv/) for dependency management

## Install uv

**macOS/Linux:**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Or via Homebrew:

```bash
brew install uv
```

## Getting started

1. Clone this repository and enter the directory:

   ```bash
   git clone <repo-url>
   cd delete-lds-configs
   ```

2. Install dependencies and launch Jupyter:

   ```bash
   uv run jupyter notebook
   ```

3. Open `edgegrid_notebook.ipynb` in the browser.

## Configuration

| Environment variable        | Description                                       | Default  |
| --------------------------- | ------------------------------------------------- | -------- |
| `AKAMAI_EDGEGRID_SECTION`   | Section name in `~/.edgerc`                       | `gss`    |
| `AKAMAI_ACCOUNT_SWITCH_KEY` | Account switch key for managing multiple accounts | _(none)_ |

Make sure the selected `AKAMAI_EDGEGRID_SECTION` has the READ/WRITE permissions for the [LDS API endpoint](https://techdocs.akamai.com/log-delivery/reference/get-started).

`AKAMAI_ACCOUNT_SWITCH_KEY` is optional. Only set it if you need to manage configurations under a different account than the one in your `~/.edgerc` file if you have the permissions.

Example:

```bash
export AKAMAI_EDGEGRID_SECTION=default
# Optional: only needed when switching account
export AKAMAI_ACCOUNT_SWITCH_KEY=F-AC-1234567:1-ABCD
uv run jupyter notebook
```

## Usage

1. Run all cells up to the **delete** cell to collect configuration IDs across all log source types (`edns`, `cpcode-products`, `gtm`, `answerx`, `etp`).
2. Review the printed list of IDs.
3. The delete cell processes only the **first config by default** as a safety check. Change `config_ids[:1]` to `config_ids` in the last cell when ready to delete all.
