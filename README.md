# SupplyChainAssurance
Reusable Workflow for SLSA Level 3

## Description

This repository includes reusable-workflows/actions that meets the requirement of SLSA Level 3, one of the solutions to prevent Supply Chain Attack.

If you use this reusable-workflow with a release event,
artifacts are created through reusable-builder.

In addition, an SBOM(Software-Bill-of-Materials) and a verified provenance are created by the scia-generator,
and these files are registered as release assets with artifacts.

An SBOM supports SPDX format and the Provenance can be verified with [slsa-verifier](https://github.com/slsa-framework/slsa-verifier)


## Limitation
Currently, we only offer the Releasor of Python Project, but we will also provide Releasors for other languages later.

To use the scia-generator, the token is required that is issued by Samsung for the open source project.

## Example Usage

Below is a sample to insert into your workflow.

```plugins
  Release:
    permissions:
      id-token: write
      contents: write
    uses: samsung/supplychainassurance/.github/workflows/python_releasor.yml@v1.0.0
    with:
      version: "3.9"
    secrets:
      EXPECTED_REPOSITORY: "${{ project repository }}"
      ECODETOKEN: "${{ secrets.token }}"
      GITHUBTOKEN: "${{ secrets.GITHUB_TOKEN }}"

```
The inputs of python_releasor are version, EXPECTED_REPOSITORY, ECODETOKEN and GITHUBTOKEN.

The version is Python's version.

EXPECTED_REPOSITORY is used to finally the repository in the provenance before release.

ECODETOKEN is used to access a private repository to get the scia_generator.
To get a token please contact us.

More details, see the [example workflow](.github/workflows/usage.sample).

### Verify
The generated provenance can be verified with slsa-verifier.
Below is a sample of command used to verify.
Replace the big quotes part with yours.

```plugins
go run ./cli/slsa-verifier verify-artifact "artifact binary name(e.g xxxx.whl)" --provenance-path "provenance path(e.g xxx.intoto.jsonl)" --source-uri "git+https://your repository" --builder-id https://github.com/samsung/supplychainassurance/.github/workflows/slsa_release.yml@v1.0.0
```

