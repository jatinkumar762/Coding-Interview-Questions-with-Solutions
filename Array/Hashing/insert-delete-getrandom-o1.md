https://leetcode.com/problems/insert-delete-getrandom-o1/description/

### Approach 1: HashMap + ArrayList

```java
class RandomizedSet {

    HashMap<Integer, Integer> valIndexMap;
    ArrayList<Integer> values;
    int size;
    Random random;

    public RandomizedSet() {
        valIndexMap = new HashMap<>();
        values = new ArrayList<>();
        size = 0;
        random = new Random();
    }

    public boolean insert(int val) {

        if (valIndexMap.containsKey(val)) {
            return false;
        }

        values.add(val); // values.add(size, val); both works 

        valIndexMap.put(val, size);

        size++;

        return true;
    }

    public boolean remove(int val) {

        if (!valIndexMap.containsKey(val)) {
            return false;
        }

        int index = valIndexMap.get(val);

        if (index == size - 1) {
            //last element
        } else {

            values.set(index, values.get(size - 1));

            valIndexMap.put(values.get(size - 1), index); //override
        }

        valIndexMap.remove(val);
        values.remove(size - 1);

        size--;

        return true;
    }

    public int getRandom() {

        return values.get(random.nextInt(size));
    }
}
```
