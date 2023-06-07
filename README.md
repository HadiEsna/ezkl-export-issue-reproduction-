# ezkl-export-issue-reproduction-
This repo contains code and steps to reproduce the issue with export function of ezkl

# to reproduce<br />
"python" folder contains the code to reproduce the issue.<br />
"python-output" folder contains the output of the python code.<br />

## Steps to reproduce
1. Install ezkl
2. Run the python code
3. check the output in "python-output" folder
4. Run "gen-srs"
```bash
mkdir ezkl & ezkl gen-srs --logrows 17 --params-path=./ezkl/kzg.params
```
5. Run "setup"
```bash
ezkl setup -M python-output/network-before-train.onnx --params-path=./ezkl/kzg.params --vk-path=./ezkl/vk.key --pk-path=./ezkl/pk.key --circuit-params-path=./ezkl/circuit.params
```

6. Run "prove" 1 (it should work)
```bash
ezkl prove -M python-output/network-before-train.onnx -D python-output/dummy-input-before-train.json --pk-path=./ezkl/pk.key --proof-path=./ezkl/model.proof --params-path=./ezkl/kzg.params --circuit-params-path=./ezkl/circuit.params
```


6. Run "prove" 2 (it fails)
```bash
ezkl prove -M python-output/network-after-train.onnx -D python-output/dummy-input-after-train.json --pk-path=./ezkl/pk.key --proof-path=./ezkl/model2.proof --params-path=./ezkl/kzg.params --circuit-params-path=./ezkl/circuit.params
```

<details>
  <summary>Click to expand the error!</summary>
  
  [*] [5s, ezkl_lib::graph::model] - model layout...
[*] [42s, ezkl_lib::graph::model] - computing...
[E] [49s, ezkl] - failed: verification failed
Error: VerifyError([ConstraintCaseDebug {
    constraint: Constraint {
        gate: Gate {
            index: 71,
            name: "RANGE",
        },
        index: 0,
        name: "",
    },
    location: InRegion {
        region: Region 2 ('model'),
        offset: 40509,
    },
    cell_values: [
        (
            DebugVirtualCell {
                name: "",
                column: DebugColumn {
                    column_type: Advice,
                    index: 17,
                    annotation: "",
                },
                rotation: 0,
            },
            "0x41",
        ),
        (
            DebugVirtualCell {
                name: "",
                column: DebugColumn {
                    column_type: Advice,
                    index: 26,
                    annotation: "",
                },
                rotation: 0,
            },
            "0x2b",
        ),
    ],
}])
</details>

6. Run "prove" 3 (it also fails)
```bash
ezkl prove -M python-output/network-after-train.onnx -D python-output/real-input-after-train.json --pk-path=./ezkl/pk.key --proof-path=./ezkl/model3.proof --params-path=./ezkl/kzg.params --circuit-params-path=./ezkl/circuit.params
```
