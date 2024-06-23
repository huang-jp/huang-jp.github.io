---
title: 冒泡和二分查找
---
a) 冒泡

    public static void mp(int a[]) {
        int swap = 0;
            for (int i = 0; i < a.length; i++) {
                for (int j = i; j < a.length; j++) {
                    if (a[j] > a[i]) {
                        swap = a[i];
                        a[i] = a[j];
                        a[j] = swap;
                }
            }
        }
        System.out.println(Arrays.toString(a));
    }
 
b)二分查找

    public static int ef(int a[], int tag) {
        int first = 0;
        int end = a.length;
        for (int i = 0; i < a.length; i++) {
            int middle = (first + end) / 2;
            if (tag == a[middle]) {
                return middle;
            }
            if (tag > a[middle]) {
                first = middle + 1;
            }
            if (tag < a[middle]) {
                end = middle - 1;
            }
        }
        return 0;
    }