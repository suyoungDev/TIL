merge sort

### merge sort 공부

- 병합정렬은 대표적인 정렬 알고리즘 중 하나
- 리스트 길이가 0 or 1이면 이미 정렬된 것.
- 길이가 그 이상이면, 정렬되지 않은 리스트의 절반을 잘라, 비슷한 크기로 나눈다
- 각 리스트를 또 비교하여 병합정렬을 이용해 정렬한다. (반복)
- 두개 리스트를 하나의 정렬된 리스트로 합친다.

[참고](https://reactgo.com/merge-sort-algorithm-javascript/)

#### 순서

1. [3, 4] [2, 1]
   - 둘로 나눈 뒤 가장 작은 chunk로 나눈다.
2. [3][4] [2][1]
3. 나눈 chunk들을 정렬하여 합친다.
   - [3, 4] [1, 2]
4. index 0 을 각각 비교했을 때, 1이 3보다 작으므로 따로 빼준다.
   - [3, 4] [2]
   - **sorted array**: [1]
5. 다시 index 0을 각각 비교하여 따로 빼준다
   - [3,4] [ ]
   - **sorted array**: [1, 2]
6. **하나의 어레이가 공백이 됐으므로 이제 합친다.**
   - [1, 2, 3, 4]

#### 실전 코드

```js
function merger(left, right) {
  // arr = sorted array
  const arr = [];
  while (left.length && right.length) {
    if (left[0] < right[0]) {
      // array의 첫번째 요소를 비교하여
      // 그중 작은 첫번째 요소를 빼낸뒤
      // sortedArray인 arr에 넣는다
      arr.push(left.shift());
    } else {
      arr.push(right.shift());
    }
  }
  return [...arr, ...left, ...right];
}

const mergeSort = (array) => {
  let half = array.length / 2;

  if (array.length < 2) {
    return array;
  }

  const left = array.splice(0, half);

  return merger(mergeSort(left), mergeSort(array));
};

const arr = [10, 2, 28, 3, 1, 9, 8, 23];
console.log(mergeSort(arr));
```

## 문제 52. quick sort

### 공부: quick sort

[참고](https://reactgo.com/quicksort-algorithm-javascript/)

- 큰 데이터일 때 merge sort나 heap sort보다 효과적인 sorting algorithm 임

#### 순서

1. 먼저 pivot (중심점)이 될만한 value를 해당 배열에서 찾는다. 아무거나 가능하다.
2. 배열을 돌면서 pivot보다 작은건 left, 큰건 right으로 나눠준다.
3. 재귀적으로 **right 부분(pivot보다 큰 파트)을 반복**한다.

#### 실전 코드

```js
const quickSort = (array) => {
  // 기초 부분
  if (array.length < 2) {
    return array;
  }

  // pivot은 아무것이나 가능하다.
  const pivot = array[array.length - 1];
  let left = [];
  let right = [];

  const len = array.length - 1;

  let index = 0;
  while (index < len) {
    if (array[index] < pivot) {
      left.push(array[index]);
    } else {
      right.push(array[index]);
    }
    index++;
  }

  // 재귀로 반복해줘야한다
  return [...quickSort(left), pivot, ...quickSort(right)];
  // pivot은 이렇게 따로 둠으로써,
  // 매번 재귀를 할때 마다 해당하는 pivot이 새로운 숫자가 된다!
};

const arr = [10, 2, 28, 3, 1, 9, 8, 23];
console.log(quickSort(arr));
```
