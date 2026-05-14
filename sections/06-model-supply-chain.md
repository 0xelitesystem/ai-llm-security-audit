# Model Supply Chain

Model weights, fine-tuning datasets, inference frameworks, and serving infrastructure all participate in the supply chain. Each is a possible attack surface.

## Risk surfaces

- **Backdoored open weights:** a published model has been trained or fine-tuned to misbehave on specific trigger inputs. Hard to detect by sampling.
- **Compromised model hubs:** typosquatted model names, modified weights with same name as legitimate, malicious "loader" scripts that execute on download.
- **Pickle deserialization:** PyTorch `.pt` and `.pkl` files are pickle-based and can execute arbitrary code on load. Many legitimate models still ship as pickle.
- **Malicious tokenizers / processors:** custom code that runs as part of `from_pretrained` calls.
- **Inference framework vulnerabilities:** the runtime stack (vLLM, TGI, llama.cpp, ONNX Runtime) has had RCE vulnerabilities. Patch promptly.
- **Compromised dependencies:** the same supply chain risks that affect any Python project (see incident-response-runbooks: supply-chain-compromise).

## Audit checklist

- Where are your models hosted? Vendor API, your own hub mirror, or downloaded from public hub at deploy time?
- Are model files verified by hash before loading? (Hugging Face provides commit hashes; pin them.)
- Are you using safetensors format where possible? (Doesn't execute code on load, unlike pickle.)
- Does your loader run with restricted permissions (no network, scoped filesystem)?
- For fine-tuned models: is the training dataset under version control with provenance? Who can write to it?
- For RAG embeddings: same questions for the embedding model.

## Specific tests

- Load a model with `weights_only=False` from an untrusted source on an isolated VM. Does anything unexpected happen?
- Check the integrity of your local model cache against published hashes.
- Audit which Python packages have access to the model loading codepath. Each is part of your trust chain.

## Mitigations

- Prefer safetensors over pickle.
- Pin models by commit hash, not by tag.
- Mirror models internally after vetting; deploy from the mirror.
- Run inference servers in network-isolated environments.
- Subscribe to security advisories for your inference stack.
- Don't run `pip install` of model dependencies as root.
