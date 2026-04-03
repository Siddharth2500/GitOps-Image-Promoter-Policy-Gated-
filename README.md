# 🚀 GitOps Image Promoter - Policy-Gated Kubernetes Image Promotion

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg?logo=python&logoColor=white)  
![Kubernetes](https://img.shields.io/badge/Kubernetes-Manifests-326CE5?logo=kubernetes)  
![CI/CD](https://img.shields.io/badge/CI/CD-Policy_Gated-2088FF?logo=githubactions)  
![GitOps](https://img.shields.io/badge/GitOps-ArgoCD/Flux-FF6F00?logo=git)  
![JUnit](https://img.shields.io/badge/Tests-JUnit_XML-25A162?logo=testcafe)  
![SupplyChain](https://img.shields.io/badge/SupplyChain-Attestations-4CAF50?logo=security)  
----------
**GitOps Image Promoter** is a **Python 3** utility designed for DevOps/GitOps workflows.  
It ensures safe container image promotions in Kubernetes manifests with policy checks:

- ✅ Enforces **minimum JUnit pass rate**  
- ✅ Verifies **supply chain attestations** (SBOM, SAST, digests)  
- ✅ Applies a **policy file** before manifest updates  
- ✅ Creates **backups** before changes  

------------------------------

## 🛠 Tech & Languages

| Layer        | Tech / Format   | Notes                                    |
|--------------|-----------------|------------------------------------------|
| Language     | **Python 3.10+**| Stdlib only, no external deps            |
| Policy       | YAML-like file  | min_pass_rate, required subjects/digests |
| Tests        | **JUnit XML**   | Enforces pass rate threshold             |
| Attestations | JSON            | SBOM, SAST, signed digests               |
| GitOps       | ArgoCD / Flux   | Syncs manifests after commit             |
| Deployment   | Kubernetes      | Manifest image tag updated in place      |

---

## 📦 Repository Structure

gitops-image-promoter/
├─ k8s/
│ └─ deploy.yaml # Kubernetes manifest
├─ policy.yml # Promotion policy
├─ attest.json # Attestations
├─ tests.xml # JUnit test results
├─ promoter.py # Promotion script
└─ README.md

python
Copy code

------

## ▶️ Run in Google Colab

```python
from promoter import promote

result = promote(
    manifest_path="k8s/deploy.yaml",
    image_repo="repo/app",
    new_tag="2.3.7",
    policy_path="policy.yml",
    attestation_path="attest.json",
    junit_path="tests.xml",
    inplace=True
)
print(result)
📄 Input Examples
Kubernetes Manifest (k8s/deploy.yaml)
yaml
Copy code
containers:
  - name: app
    image: repo/app:1.2.3
Policy (policy.yml)
yaml
Copy code
min_pass_rate: 0.95
require_subjects:
  - sbom
  - sast
require_digests:
  - sha256:1234deadbeef
Attestation (attest.json)
json
Copy code
{
  "subjects": ["sbom", "sast", "sign"],
  "digests": ["sha256:1234deadbeef"]
}
JUnit Report (tests.xml)
xml
Copy code
<testsuite tests="10" failures="0" errors="0" name="unit"/>
🧪 Example Output
json
Copy code
{
  "ok": true,
  "changed": true,
  "backup": "k8s/deploy.backup.20251002T075544Z.yaml",
  "manifest_hash": "facc9189d29a",
  "new_image_line": "image: repo/app:2.3.7",
  "pass_rate": 1.0
}
🐳 Docker
Build:

bash
Copy code
docker build -t gitops-promoter:latest .
Run:

bash
Copy code
docker run --rm -v $(pwd):/work gitops-promoter:latest \
  python promoter.py --manifest k8s/deploy.yaml ...
☸️ GitOps Integration
bash
Copy code
git add k8s/deploy.yaml
git commit -m "Promote repo/app:2.3.7 via policy gate"
git push
ArgoCD or Flux will detect and sync automatically.

📊 Use Cases
Enforce test quality gates before production

Require attestations for compliance

Automate safe promotions in CI/CD pipelines

Run demos in Google Colab with zero dependencies

-------

## 👤 Author
Siddharth Raut — DevOps / Platform Engineer

📧 Email: siduk2500@gmail.com

💼 LinkedIn: linkedin.com/in/siddharth-raut-

