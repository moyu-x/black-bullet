# 数组

## 1、两数之和

### 方法一 暴力循环法

直接利用两次循环解决问题

### 方法二 两次哈希

1. 遍历数组，构建Map，下标作为value
2. 再次遍历数组，用target减去当前值做键去寻找数据，要排除获取的数组下标就是当前值的情况

### 方法三 一次哈希

从哈希表中取出的数据就是正确答案，否则无答案：

```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> targetMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int key = target - nums[i];
            if (Objects.nonNull(targetMap.get(key))) {
                return new int[]{i, targetMap.get(key)};
            }
            targetMap.put(nums[i], i);
        }

        throw new IllegalArgumentException("错了啊");
    }
```

## 6、Z字形变化

![](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fec8fdb-945a-4334-a4d6-04e0a52712ed/Untitled.png)

由上图可以看出，以每个竖列加中间数据作为一组，可以确定每组可以放$2n-2$个数据，这个时候就能取模获得在数组中的位置

```java
    public String convert(String s, int numRows) {
        // 处理边界
        if (1 == numRows) {
            return s;
        }

        String[] result = new String[numRows];

        // 默认数组初始会以null填充
        Arrays.fill(result, "");

        char[] stTemp = s.toCharArray();
        int period = numRows * 2 - 2;

        for (int i = 0; i < stTemp.length; i++) {
            int mod = i % period;
            // 在row里面，直接存指定位置
            if (mod < numRows) {
                result[mod] += stTemp[i];
            } else {
                // 已经超过，直接减去就能找到正确位置
                result[period - mod] += stTemp[i];
            }

        }

        return String.join("", result);
    }
```

## 14、最长公共子前缀

初始化结果为默认的第一个字符串，对给定的字符串数组进行遍历，每次对字符串中的字符与结果字符串的相同位置进行比较，遇到不同则截断字符串，最后返回（要处理给定字符串数组为空的边界条件）

相当于每次都是当前字符串与结果字符中的字符一一比较

## 15、三数之和

1. 先进行排序
2. 遍历数据，每次固定当前值，计算剩下两个数的目标和
3. 对接下来的数据进行双针扫描，如果结果比计算值少，则移动左指针，扩大数据；结果过大，则移动右指针

```java
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        ArrayList<List<Integer>> result = new ArrayList<>();
        // 固定尾部，移动头部
        for (int i = 0; i < nums.length; i++) {
            int value = -nums[i];
            int lp = i + 1;
            int rp = nums.length - 1;
            // 因为排序，当这个只大于0就没有继续的必要
            if (0 < nums[i]) {
                break;
            }
            // 数据在一起
            if (lp != rp && (i == 0 || nums[i] != nums[i - 1])) {
                while (lp < rp) {
                    if (nums[lp] + nums[rp] == value) {
                        result.add(Arrays.asList(nums[i], nums[lp], nums[rp]));
                        lp++;
                        rp--;
                        while (lp < rp && nums[lp] == nums[lp -1]) {
                            lp++;
                        }
                        while (lp < rp && nums[rp] == nums[rp + 1]) {
                            rp--;
                        }

                    } else if (nums[lp] + nums[rp] < value) {
                        lp++;
                    } else {
                        rp--;
                    }
                }
            }
        }

        return result;
    }
```

## 26、删除排序数组中的重复项

双针法解决，一个指针表示当前已经不重复的元素的位置，另外一个为遍历指针，数据相同移动遍历指针，数据不同交换数据

```java
    public int removeDuplicates(int[] nums) {

        int interval = 0;

        for (int i = 1; i < nums.length;) {
           if (nums[interval] == nums[i]) {
               i +=1;
           } else {
               nums[interval + 1] = nums[i];
               interval +=1;
           }
        }
        return interval + 1;
    }
```

## 27、原地删除

1. 遍历数组，每次都将当前数据移动到正确位置
2. 并记录出现关键值的次数
3. 最后数组长度减去关键值次数并返回

```java
    public int removeElement(int[] nums, int val) {
        int interval = 0;

        for (int i = 0; i < nums.length; i++) {
            if (val == nums[i]) {
                interval += 1;
            }else {
                nums[i - interval] = nums[i];
            }
        }
        return nums.length - interval;
    }
```

## 66、加一

三种情况：

1. 末尾加一不用进位
2. 末尾加一只用进一位
3. 全是9的情况

解决方法如下：

1. 申明进位数据，默认为0，因为刚开始的时候不用进位
2. 处理末尾自增
3. 处理自增后还需要进位的情况
4. 处理首位要自增构建新数组的情况

```java
    public int[] plusOne(int[] digits) {
        int addon = 0;
        for (int i = digits.length - 1; i >= 0; i--) {
            digits[i] += addon;
            addon = 0;

            // 先处理最后一位
            if (digits.length - 1 == i) {
                digits[i] += 1;
            }

            // 处理需要进位的情况
            if (digits[i] == 10) {
                addon = 1;
                digits[i] %= 10;
            }
        }

        // 处理最后首位需要进位的情况
        if (1 != addon) {
            return digits;
        }

        int[] result = new int[digits.length + 1];
        result[0] = 1;
        System.arraycopy(result, 1, digits, 0, digits.length);
        return result;
    }
```

## 122、买卖股票的最佳时机II

最中要的限制就是不能在同一天买入和卖出，所以这个时候在画出每一天的数据的折线图后，就有以下解决办法：

### 方法一 连续递增的端点

此方法认为在递增段上获取利润，所以

1. 遍历给定数据
2. 遇到增长就计算利润：

```java
for (int i = 1; i < prices.length; i++) {
    if (prices[i] > prices[i -1]) {
        maxprofit += prices[i] - prices[i - 1];
    }
}
```

### 方法二 计算每次波谷和波峰的利润

就是每次先波谷买入，波峰卖出:

1. 遍历数据
    1. 先找波谷
    2. 再找波峰
    3. 波峰 - 波谷
2. 重复以上过程

```java
for (int i = 0; i < prices.length; i++) {
    // 计算波谷
    while (i < prices.length - 1 && prices[i] >= prices[i + 1]) {
        i++;
    }
    int minIndex = i;

    // 计算波峰
    while (i < prices.length - 1 && prices[i] <= prices[i + 1]) {
        i++;
    }
    int maxIndex = i;

    maxprofit += prices[maxIndex] - prices[minIndex];
}
```

## 189、旋转数组

两个前置条件：

1. 数组长度为空或者一的时候直接返回原数组
2. 当移动长度大于数组长度的时候要以数组长度取模`k % nums.length`

### 方法一 反转数组

移动数组k次，相当于将数组尾部的k个元素移动到数组头部，所以方法如下：

1. 反转整个数组
2. 反正0到k个数组元素
3. 反正k后的数组元素

反转数组的代码如下：

```java
public void reverse(int start, int end, int[] nums) {
    for (int i = 0; i < (end - start) / 2; i++) {
        nums[i + start] += nums[end - i - 1];
        nums[end - i - 1] = nums[i + start] - nums[end - i - 1];
        nums[i + start] = nums[i + start] - nums[end -i -1];
    }
}
```

### 方法二 环状替代

直接交换两个元素，当形成环的时候结束，如果还有没有交换过的元素，则偏移后进行下一次循环

```java
int count = 0;
for (int i = 0; count < nums.length; i++) {
    int current = i;
    int prevValue = nums[current];
    do  {
        int prev = (current + k) % nums.length;

        int temp = nums[prev];
        nums[prev] = prevValue;
        prevValue = temp;

        current = prev;
        count++;
    } while (i != current);
}
```

## 350、两数组交集II

### 方法一 构建Map的方式

1. 交换两数组，将长度小的交换的到前面（减少空间）
2. 初始化Map并遍历第一个数组，并统计每个数字出现的频率
3. 遍历第二个数组，并与Map的键值对比，不存在继续；若存在，则输出到第一个数组种，并让下标index偏移

### 方法二 交替遍历

1. 对两个数组进行排序
    
2. 对两个数组交替进行遍历，遇到相同则记录，核心代码如下
    
    ```java
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] > nums2[j]) {
            j++;
        } else if (nums1[i] < nums2[j]) {
            i++;
        } else {
            nums1[k] = nums1[i];
            i++;
            j++;
            k++;
        }
    }
    ```