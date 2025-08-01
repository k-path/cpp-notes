# Stable Sort
In C++, `std::stable_sort` is a function template provided in the <algorithm> header that sorts elements within a specified range while preserving the relative order of equivalent elements. 

Unlike `std::sort`, which does not guarantee the relative order of elements that compare as equal, `std::stable_sort` ensures that if two elements are considered equivalent by the comparison criterion, their original relative order in the sequence is maintained after sorting.
```
stable_sort(len_and_word.begin(), len_and_word.end(), [](const pair<int, string>& a, const pair<int, string>& b) { return a.first < b.first;} );
```
Note that stable_sort requires you to have const on the lambda function's parameters.
