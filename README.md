# Algorithm
// ===================== SEARCHING =====================

// Linear search: works on unsorted data. O(n)
function linearSearch(a, key) {
  for (let i = 0; i < a.length; i++) {
    if (a[i] === key) return i;
  }
  return -1;
}

// Binary search: requires SORTED data. O(log n)
function binarySearch(a, key) {
  let lo = 0, hi = a.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (a[mid] === key) return mid;
    if (a[mid] < key) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}

// ===================== SORTING =====================

// Bubble sort: swap adjacent out-of-order pairs. Best O(n), worst O(n^2)
function bubbleSort(a) {
  const n = a.length;
  for (let i = 0; i < n - 1; i++) {
    let swapped = false;
    for (let j = 0; j < n - 1 - i; j++) {
      if (a[j] > a[j + 1]) {
        [a[j], a[j + 1]] = [a[j + 1], a[j]];
        swapped = true;
      }
    }
    if (!swapped) break; // already sorted
  }
  return a;
}

// Selection sort: pick the minimum, place it in front. O(n^2), max n-1 swaps
function selectionSort(a) {
  for (let i = 0; i < a.length - 1; i++) {
    let min = i;
    for (let j = i + 1; j < a.length; j++) {
      if (a[j] < a[min]) min = j;
    }
    [a[i], a[min]] = [a[min], a[i]];
  }
  return a;
}

// Insertion sort: insert each item into the sorted left part. Best O(n)
function insertionSort(a) {
  for (let i = 1; i < a.length; i++) {
    const key = a[i];
    let j = i - 1;
    while (j >= 0 && a[j] > key) {
      a[j + 1] = a[j];
      j--;
    }
    a[j + 1] = key;
  }
  return a;
}

// Merge sort: divide, sort halves, merge. Always O(n log n), stable, O(n) space
function mergeSort(a) {
  if (a.length <= 1) return a;
  const mid = Math.floor(a.length / 2);
  const left = mergeSort(a.slice(0, mid));
  const right = mergeSort(a.slice(mid));
  const out = [];
  let i = 0, j = 0;
  while (i < left.length && j < right.length) {
    out.push(left[i] <= right[j] ? left[i++] : right[j++]);
  }
  return out.concat(left.slice(i), right.slice(j));
}

// Quick sort: partition around a pivot. Avg O(n log n), worst O(n^2)
function quickSort(a, lo = 0, hi = a.length - 1) {
  if (lo < hi) {
    const p = partition(a, lo, hi);
    quickSort(a, lo, p - 1);
    quickSort(a, p + 1, hi);
  }
  return a;
}

function partition(a, lo, hi) {
  const pivot = a[hi];
  let i = lo - 1;
  for (let j = lo; j < hi; j++) {
    if (a[j] <= pivot) {
      i++;
      [a[i], a[j]] = [a[j], a[i]];
    }
  }
  [a[i + 1], a[hi]] = [a[hi], a[i + 1]];
  return i + 1;
}

// Heap sort: build a max-heap, repeatedly move the max to the end. O(n log n)
function heapSort(a) {
  const n = a.length;
  for (let i = Math.floor(n / 2) - 1; i >= 0; i--) heapify(a, n, i); // build heap: O(n)
  for (let end = n - 1; end > 0; end--) {
    [a[0], a[end]] = [a[end], a[0]];
    heapify(a, end, 0);
  }
  return a;
}

function heapify(a, n, i) {
  let largest = i;
  const l = 2 * i + 1, r = 2 * i + 2;
  if (l < n && a[l] > a[largest]) largest = l;
  if (r < n && a[r] > a[largest]) largest = r;
  if (largest !== i) {
    [a[i], a[largest]] = [a[largest], a[i]];
    heapify(a, n, largest);
  }
}

// ===================== QUICK TEST =====================
const data = [38, 27, 43, 3, 9, 82, 10];
console.log("Bubble   :", bubbleSort([...data]));
console.log("Selection:", selectionSort([...data]));
console.log("Insertion:", insertionSort([...data]));
console.log("Merge    :", mergeSort([...data]));
console.log("Quick    :", quickSort([...data]));
console.log("Heap     :", heapSort([...data]));

const sorted = mergeSort([...data]);
console.log("Linear search 43 :", linearSearch(data, 43));
console.log("Binary search 43 :", binarySearch(sorted, 43));
