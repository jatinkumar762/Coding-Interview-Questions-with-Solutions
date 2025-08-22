* https://leetcode.com/problems/combination-sum/description/

- Given an array of **distinct** integers candidates 

```java
class Solution {

    private List<List<Integer>> result = new ArrayList<>();
    int N;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        //Given an array of distinct integers candidates
        //so duplicates will not happen
        N = candidates.length;
        findCombinationSum(0, candidates, target, new ArrayList<>());
        return result;
    }

    private void findCombinationSum(int start, int[] candidates, int target, List<Integer> tmp) {

        if (target < 0)
            return;

        if (target == 0) {
            result.add(new ArrayList<>(tmp));
            return;
        }

        for (int i = start; i < N; i++) {
            if (target >= candidates[i]) {
                tmp.add(candidates[i]);
                findCombinationSum(i, candidates, target - candidates[i], tmp);
                tmp.remove(tmp.size() - 1);
            }
        }

    }
}
```


---

```java
class Solution {

    private List<List<Integer>> result;
    private int len;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {

        result = new ArrayList<>();
        len = candidates.length;

        findCombination(candidates, 0, target, new ArrayList<>());

        return result;
    }

    private void findCombination(int[] candidates, int start, int target, List<Integer> tmp) {

        if (target < 0 || start == len) {
            return;
        }

        if (target == 0) {
            result.add(new ArrayList<>(tmp));
            return;
        }

        if (candidates[start] <= target) {
            tmp.add(candidates[start]);
            findCombination(candidates, start, target - candidates[start], tmp);
            tmp.remove(tmp.size() - 1);
        }

        findCombination(candidates, start + 1, target, tmp);
    }
}
```