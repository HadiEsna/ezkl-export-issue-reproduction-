# ezkl-export-issue-reproduction-
This repo contains code and steps to reproduce the issue with export function of ezkl

* to reproduce
"python" folder contains the code to reproduce the issue.
"python-output" folder contains the output of the python code.

** Steps to reproduce
1. Install ezkl
2. Run the python code
3. Move the output to the "python-output" folder
4. Run "gen-srs"
```bash
mkdir ezkl & ezkl gen-srs --logrows 23 --params-path=./ezkl/kzg.params
```
5. Run "setup"
```bash
ezkl setup -M python-output/network-before-train.onnx --params-path=./ezkl/kzg.params --vk-path=./ezkl/vk.key --pk-path=./ezkl/pk.key --circuit-params-path=./ezkl/circuit.params
```
6. Run "prove"
```bash
ezkl prove -M python-output/network-before-train.onnx -D python-output/dummy-input-before-train.json --pk-path=./ezkl/pk.key --proof-path=./ezkl/model.proof --params-path=./ezkl/kzg.params --circuit-params-path=./ezkl/circuit.params
```