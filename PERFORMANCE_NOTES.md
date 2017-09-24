## Graph/helper.jsx


// Benchmark (10 runs mean)
// New Impl.: 0.222412109375 (with graphNodes.reduce)
// New Impl.: 0.1168212890625 (with good old for loop)
// Old Impl.: 0.181982421875

### function initializeNodes(graphNodes)

#### master

0.3310546875 + 0.185791015625 + 0.14599609375 + 0.180908203125 + 0.165771484375 + 0.139892578125 + 0.166015625 + 0.166259765625 + 0.168212890625 + 0.169921875

#### graphNodes.reduce

| Run           | Time (in ms)   |
| ------------- |:--------------:|
| 1             | 0.290771484375 |
| 2             | 0.164794921875 |
| 3             | 0.255859375    |
| 4             | 0.219970703125 |
| 5             | 0.219970703125 |
| 6             | 0.1787109375   |
| 7             | 0.18798828125  |
| 8             | 0.244873046875 |
| 9             | 0.22607421875  |
| 10            | 0.235107421875 |
| **Mean**      | **0.222412109375** |

```javascript
console.time();

let index = 0;

const r = graphNodes.reduce((acc, n) => {
    n['highlighted'] = false;
    if (!n.hasOwnProperty('x')) n['x'] = 0;
    if (!n.hasOwnProperty('y')) n['y'] = 0;

    acc.nodes[n.id.toString()] = n;
    acc.nodeIndexMapping[index] = n.id;

    index++;

    return acc;
}, {nodes: {}, nodeIndexMapping: {}});

console.timeEnd();

return r;
```

#### good old for loop

0.113037109375 + 0.121826171875 + 0.1171875 + 0.10400390625 + 0.110107421875 + 0.116943359375 + 0.108154296875 + 0.14697265625 + 0.114013671875 + 0.115966796875

```javascript
console.time();
let nodes = {};
let nodeIndexMapping = {};

const n = graphNodes.length;

for (let i=0; i < n; i++) {
    const node = graphNodes[i];

    node['highlighted'] = false;

    if (!n.hasOwnProperty('x')) { node['x'] = 0 };
    if (!n.hasOwnProperty('y')) { node['y'] = 0 };

    nodes[node.id.toString()] = node;
    nodeIndexMapping[i] = node.id;
}

console.timeEnd();

return {
    nodes,
    nodeIndexMapping
};
```