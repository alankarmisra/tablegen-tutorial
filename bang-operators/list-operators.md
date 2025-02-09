# List operators

```c
/// listops.td

#ifndef LISTOPS
#define LISTOPS

include "setup.td"

def ListOps {
    // Lists and related operations
  list<int> lst_1 = [10, 20];
  list<int> lst_2 = [30, 40];
  
  // !listconcat(list1, list2, ...) // any number of lists allowed
  list<int> lst_c = !listconcat(lst_1, lst_2); // [10, 20, 30, 40]
  assert !eq(!repr(lst_c), "[10, 20, 30, 40]"), errorStr;

  // !listremove(list1, list2) // remove elements of list2 from list1 
  list<int> lst_r = !listremove(lst_c, lst_2); // [10, 20]
  assert !eq(!repr(lst_r), "[10, 20]"), errorStr;
  
  // !head(list) - Produces the zeroth element of the list a
  int h = !head(lst_c); // 10
  assert !eq(h, 10), errorStr;

  // !tail(list) - Removes the head of the list
  list<int> t = !tail(lst_c); // [20, 30, 40] 
  assert !eq(!repr(t), "[20, 30, 40]"), errorStr;
  
  // "Splat" means replicating a value multiple times to fill a larger structure. 
  // Here, the value 1 is repeated 3 times to create the list [1, 1, 1].
  list<int> lst_s = !listsplat(1, 3); // [1, 1, 1]
  assert !eq(!repr(lst_s), "[1, 1, 1]"), errorStr;
  
  // !foreach(var, sequence, expr)
  list<int> foreach_v = !foreach(px, [1,2,3], !mul(px,2)); // [2, 4, 6]
  assert !eq(!repr(foreach_v), "[2, 4, 6]"), errorStr;
  
  // range([start,] end [, step]) - produces a list of numbers [start, end) i.e. end is not included. 
  list<int> r1 = !range(4); // == !range(0, 4, 1) == [0, 1, 2, 3]
  assert !eq(!repr(r1), "[0, 1, 2, 3]"), errorStr;

  list<int> r2 = !range(1, 4); // !range(1, 4, 1) == [1, 2, 3]
  assert !eq(!repr(r2), "[1, 2, 3]"), errorStr;

  list<int> r3 = !range(0, 4, 2); // [0, 2]
  assert !eq(!repr(r3), "[0, 2]"), errorStr;

  list<int> r4 = !range(4, 1, -2); // [4, 2]
  assert !eq(!repr(r4), "[4, 2]"), errorStr;

  list<int> r5 = !range(0, 4, -1); // []
  assert !eq(!repr(r5), "[]"), errorStr;

  list<int> r6 = !range(4, 0, 1); // []
  assert !eq(!repr(r6), "[]"), errorStr;
  
  // !foldl(init, list, acc, var, expr)
  // Note: It's fold-left - hence foldl and not just fold. 
  int sum_v = !foldl(0, [1, 1, 1, 1], total, rec, !add(total, rec)); // 4
  assert !eq(sum_v, 4), errorStr;

  string op_string = !foldl("", 
    [addOp, mulOp, divOp], str, op, 
    !strconcat(str, ", ", op.name)); // ", add, mul, div"
  assert !eq(op_string, ", add, mul, div"), errorStr;
  
  // !filter(var, list, predicate)
  // var is how you reference the list item in the predicate.
  list<int> filteredList = !filter(x, [1, 2, 3, 4, 5, 6], !eq(x, 3)); // [3]
  assert !eq(!repr(filteredList), "[3]"), errorStr;

  list<Op> filteredRecList = !filter(op, [addOp, mulOp, divOp], !eq(op.name, "add")); // [addOp]
  assert !eq(filteredRecList[0], addOp), errorStr;
  
  assert !empty([]<int>), errorStr;
  assert !empty(r5), errorStr;  
  assert !empty(r6), errorStr;
 
  // !range(list) == !range(0, !size(list))
  list<int> charIndices = !range(["a","b","c"]); // [0, 1, 2]
  assert !eq(!repr(charIndices), "[0, 1, 2]"), errorStr;
}

#endif // LISTOPS
```
