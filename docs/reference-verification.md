# Reference verification

These checks examine documentation and selected RGDS contract boundaries. They preserve the original records and test modified copies in memory. [Existing validation scope](validation.md), [contract checker](../scripts/check_rgds_contract.py).

## Environment

Use Python 3.11, matching the [workflow](../.github/workflows/validate.yml). The documentation checker supports Python 3.9 or later. The contract checker uses `jsonschema[format]==4.25.1`. [Requirements](validation.md).

```sh
python3.11 -m venv .venv
. .venv/bin/activate
python -m pip install 'jsonschema[format]==4.25.1'
```

## Check documentation

```sh
python scripts/check_docs.py
```

Expected output: `PASS` with the inspected local-link count and 17 required inventory files. The count of links can change as documentation grows. This script checks local Markdown targets, simple anchors and selected wording boundaries. [Checker](../scripts/check_docs.py).

## Check the pinned RGDS contract

Create a fresh reference checkout. The validator requires exactly commit `f6fa066c7e53d1d89d68ac8ef424b559ef58be34`, published as RGDS v2.0.1. [Pin and checks](../scripts/check_rgds_contract.py), [CI checkout](../.github/workflows/validate.yml).

```sh
reference_dir=$(mktemp -d)
git clone https://github.com/node-and-norm/rgds.git "$reference_dir/rgds"
git -C "$reference_dir/rgds" checkout --detach f6fa066c7e53d1d89d68ac8ef424b559ef58be34
python scripts/check_rgds_contract.py "$reference_dir/rgds"
```

Expected output:

```text
PASS: pinned baseline, disclosure deletion, semantic requirements, optional fields, warning boundaries
These checks do not assess human authorship, substantive independence, or practical influence.
```

The checker confirms the canonical example, required disclosure fields, optional fields and selected warning behavior. It also demonstrates that a false AI-use declaration can retain a valid schema shape. Structural acceptance therefore leaves historical truth and substantive review open. [Checker](../scripts/check_rgds_contract.py), [dependency procedure](ai-dependency-test.md).

## Human review

Review the material rationale against inspectable sources. Record unsupported dependencies and keep structural and substantive findings separate. The illustrative case remains hypothetical and its independence assessment remains indeterminate. [Procedure](ai-dependency-test.md), [example](../examples/rgds-dec-0003-ai-assisted-conditional-go.md).
