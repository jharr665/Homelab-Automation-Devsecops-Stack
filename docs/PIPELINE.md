# CI/CD Pipeline

- GitHub Actions workflow builds, scans, signs, and attests images.
- SBOM generated via Syft.
- Vulnerability scan with Trivy.
- Signing and attestation with Cosign.
- Kyverno in cluster verifies signatures before deploy.
