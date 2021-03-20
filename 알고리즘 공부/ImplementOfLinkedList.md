# 이게 뭐고 언제 써야할까?

- Abstack data type
- Sequence of Nodes
- Can NOT access items directly
- Can be single or doubly
- Can be circular and sorted
- Follows and order
- Can be used to implement other data structures like Stack & Queue

## Linked List

```js
class LinkedList {
  size = 0;
  head = null;

  get isEmpty() {
    return this.size === 0;
  }

  createNode(element) {
    return { element, next: null };
  }

  remove(index = 0) {
    if (index < 0 || index >= this.size) return null;

    let removedNode = this.head;
    if (index === 0) {
      this.head = this.head.next;
    } else {
      let previous = this.getNodeAt(index - 1);
      removedNode = this.getNodeAt(index);
      previous.next = removedNode.next;
    }

    this.size--;
    return removedNode.element;
  }

  push(element) {
    const node = this.createNode(element);

    if (this.head === null) {
      this.head = node;
    } else {
      let current = this.getNodeAt(this.size - 1);
      current.next = node;
    }

    return this.size++;
  }

  toString() {
    if (!this.size) return '';
    let str = `${this.head.element}`;
    let current = this.head;

    for (let i = 0; i < this.size - 1; i++) {
      current = current.next;
      str += ` => ${current.element}`;
    }

    return str;
  }

  print() {
    let arr = [];
    if (this.size) {
      let current = this.head;
      for (let i = 0; i < this.size; i++) {
        arr.push(current);
        current = current.next;
      }
    }
    console.log(arr);
  }

  insert(element, index = 0) {
    if (index < 0 || index > this.size) return false;

    const node = this.createNode(element);

    if (index === 0) {
      node.next = this.head;
      this.head = node;
    } else {
      let current = this.getNodeAt(index - 1);
      node.next = current.next;
      current.next = node;
    }

    this.size++;
  }

  getNodeAt(index) {
    if (index > this.size || index < 0 || index == undefined) return null;
    let current = this.head;
    for (let i = 0; i < index; i++) {
      current = current.next;
    }
    return current;
  }

  get(index) {
    const node = this.getNodeAt(index);
    return node ? node.element : null;
  }

  indexOf(element) {
    let current = this.head;

    for (let i = 0; i < this.size; i++) {
      if (current.element === element) return i;
      current = current.next;
    }
    return -1;
  }

  contains(element) {
    return this.indexOf(element) !== -1;
  }
}
```

## Double Linked List

```js
class DoubleLinkedList extends LinkedList {
  tail = null;

  createNode(element) {
    return { element, next: null, previous: null };
  }

  push(element) {
    const node = this.createNode(element);

    if (this.head === null) {
      this.head = node;
    } else {
      let current = this.getNodeAt(this.size - 1);
      current.next = node;
      node.previous = current;
    }

    this.tail = node;

    return (this.size += 1);
  }

  insert(element, index = 0) {
    if (index < 0 || index > this.size) return false;

    const node = this.createNode(element);

    if (index === 0) {
      if (this.head) {
        node.next = this.head;
        this.head.previous = null;
      } else {
        this.tail = node;
      }
      this.head = node;
    } else if (index === this.size) {
      node.previous = this.tail;
      this.tail.next = node;
      this.tail = node;
    } else {
      let current = this.getNodeAt(index);
      let prev = current.prev;

      prev.next = node;
      current.previous = node;
      node.previous = prev;
      node.next = current;
    }

    return (this.size += 1);
  }

  remove(index = 0) {
    if (index < 0 || index >= this.size) return null;

    let removedNode = this.head;

    if (index === 0) {
      this.head = removedNode.next;
      if (this.size === 1) {
        this.tail = null;
      } else {
        this.head.previous = null;
      }
    } else if (index === this.size - 1) {
      removedNode = this.tail;
      this.tail = removedNode.previous;
      this.tail.next = null;
    } else {
      removedNode = this.getNodeAt(index);
      let prev = removedNode.previous;
      let nextNode = removedNode.next;

      prev.next = nextNode;
      nextNode.previous = prev;
    }

    this.size -= 1;
    return removedNode.element;
  }

  toString() {
    if (!this.size) return '';
    let str = `${this.head.element}`;
    let current = this.head;

    for (let i = 0; i < this.size - 1; i++) {
      current = current.next;
      str += ` <=> ${current.element}`;
    }

    return str;
  }

  reverse() {
    let current = this.head;
    this.head = this.tail;
    this.tail = current;

    for (let i = 0; i < this.size; i++) {
      const { previous, next } = current;
      current.next = previous;
      current.previous = next;
      current = next;
    }
  }
}
```

## CircluarLinkedList

```js
class CircluarLinkedList extends LinkedList {
  push(element) {
    const node = this.createNode(element);

    if (!this.head) {
      this.head = node;
    } else {
      let current = this.getNodeAt(this.size - 1);
      current.next = node;
    }

    node.next = this.head;

    return (this.size += 1);
  }

  insert(element, index = 0) {
    if (index < 0 || index > this.size) return false;

    const node = this.createNode(element);

    if (index === 0) {
      node.next = this.head;

      if (this.isEmpty) {
        node.next = node;
      } else {
        const last = this.getNodeAt(this.size - 1);
        last.next = node;
      }

      this.head = node;
    } else if (index === this.size) {
      let current = this.getNodeAt(index - 1);
      current.next = node;
      node.next = this.head;
    } else {
      let current = this.getNodeAt(index - 1);
      node.next = current.next;
      current.next = node;
    }

    this.size++;
  }

  remove(index = 0) {
    if (index < 0 || index > this.size) return null;

    let removedNode = this.head;

    if (index === 0) {
      if (this.size === 1) {
        this.head = null;
      } else {
        const last = this.getNodeAt(this.size - 1);
        this.head = this.head.next;
        last.next = this.head;
      }
    } else {
      let previous = this.getNodeAt(index - 1);
      removedNode = this.getNodeAt(index);
      previous.next = removedNode.next;
    }

    this.size--;
    return removedNode.element;
  }
}
```

## CircluarDoubleLinkedList

```js
class CircluarDoubleLinkedList extends DoubleLinkedList {
  push(element) {
    const node = this.createNode(element);

    if (!this.head) {
      this.head = node;
    } else {
      let current = this.getNodeAt(this.size - 1);
      current.next = node;
      node.prev = current;
    }

    this.tail = node;
    this.tail.next = this.head;

    return (this.size += 1);
  }

  insert(element, index = 0) {
    if (index < 0 || index > this.size) return false;

    const node = this.createNode(element);

    if (index === 0) {
      if (this.head) {
        node.next = this.head;
        this.head.previous = null;
      } else {
        this.tail = node;
      }
      this.head = node;
      this.tail.next = this.head;
    } else if (index === this.size) {
      this.tail.next = node;
      node.prev = this.tail;
      node.next = this.head;
      this.tail = node;
    } else {
      let current = this.getNodeAt(index);
      let prev = current.prev;

      prev.next = node;
      current.previous = node;
      node.previous = prev;
      node.next = current;
    }

    return (this.size += 1);
  }

  remove(index = 0) {
    if (index < 0 || index >= this.size) return null;

    let removedNode = this.head;

    if (index === 0) {
      this.head = removedNode.next;
      if (this.size === 1) {
        this.tail = null;
      } else {
        this.head.previous = null;
        this.tail.next = this.head;
      }
    } else if (index === this.size - 1) {
      removedNode = this.tail;
      this.tail = removedNode.previous;
      this.tail.next = this.head;
    } else {
      removedNode = this.getNodeAt(index);
      let prev = removedNode.previous;
      let nextNode = removedNode.next;

      prev.next = nextNode;
      nextNode.previous = prev;
    }

    this.size -= 1;
    return removedNode.element;
  }
}
```
