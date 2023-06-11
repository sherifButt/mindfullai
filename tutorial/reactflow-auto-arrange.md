# Arranging nodes in a diagram to follow a side way
Arranging nodes in a diagram to follow a side way structure or tree-like pattern with rectangular edges can be accomplished in React Flow using a combination of the library's node layout functionality and the `smoothstep` edge type.

React Flow doesn't currently provide a built-in method for automatic side way or tree layout of nodes. But you can calculate the positions of nodes in a tree structure yourself. 

Here's an example of how you might do this, creating a binary tree:

```javascript
import React from 'react';
import ReactFlow, { addEdge, removeElements, Controls, Background } from 'react-flow-renderer';

const createNodeTree = (rootId, depth = 3, spread = 2, distance = 150) => {
    let elements = [];
    let idCounter = 0;
    
    const createNode = (id, x = 0, y = 0) => ({
        id: `node_${id}`,
        type: 'input',
        position: { x, y },
        data: { label: `Node ${id}` },
    });
    
    const createEdge = (sourceId, targetId) => ({
        id: `edge_${sourceId}_${targetId}`,
        source: `node_${sourceId}`,
        target: `node_${targetId}`,
        animated: true,
        type: 'smoothstep',
    });

    const buildTree = (nodeId, level = 0) => {
        if (level === depth) return;

        const leftChildId = ++idCounter;
        const rightChildId = ++idCounter;

        elements.push(createNode(leftChildId, nodeId * spread * distance, (level + 1) * distance));
        elements.push(createNode(rightChildId, (nodeId + 1) * spread * distance, (level + 1) * distance));
        
        elements.push(createEdge(nodeId, leftChildId));
        elements.push(createEdge(nodeId, rightChildId));

        buildTree(leftChildId, level + 1);
        buildTree(rightChildId, level + 1);
    };
    
    elements.push(createNode(rootId)); // add root
    buildTree(rootId); // start building the tree from root

    return elements;
}

const FlowTree = () => {
    const elements = createNodeTree(0);

    return (
        <ReactFlow
            elements={elements}
            style={{ width: '100%', height: '800px' }}
        >
            <Background variant="dots" gap={12} size={1} />
            <Controls />
        </ReactFlow>
    );
};

export default FlowTree;
```
In this example, `createNodeTree` function generates a binary tree layout by recursively creating left and right child nodes for each node, positioning them at a certain distance from their parent. The edges are created as `smoothstep` type for rectangular edges. 

The `depth`, `spread`, and `distance` parameters control the tree's depth, horizontal spacing, and vertical spacing between nodes, respectively.

Remember that this is a basic example and you might need to adjust it to suit your particular needs or the data you're working with.