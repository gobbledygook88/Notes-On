Sorting
=======

Here we present a list of sorting algorithms and sample pseudocode. Arrays are zero-base indexed.

- Insertion Sort

        input:  array A
        output: sorted array A

        for i=1 to i=length(A)-1
            value = A[i]
            place = i
            while place>0 and A[place-1]>value
                A[place] = A[place-1]
                place    = place-1
            A[place] = value

- Selection Sort

        input:  array A
        output: sorted array A

        for i=0 to i=length(A)-1
            place = i
            min = A[i]
            for j=i+1 to j=length(A)-1
                if A[j]<min
                    place = j
                    min = A[j]
            swap(A[i],A[j])

- Bubble Sort

        // Usual Implementation
        input:  array A
        output: sorted array A

        do
            swapped = false
            for i=0 to i=length(A)-1
                if A[i-1]>A[i]
                    swap(A[i-1],A[i])
                    swapped = true
        while !swapped

        // 50% less comparisons
        n=length(A)
        do
            newn=0
            for i=0 to i=n-1
                if A[i-1]>A[i]
                    swap(A[i-1],A[i])
                    newn=i
            n=newn
        while n!=0

- Merge Sort

        input:  array A
        output: sorted array A

        merge_sort(array A):
            if length(A)<=1
                return A
            else
                declare arrays left,right
                middle = length(A)/2                            // Integer division
                for each x in A before middle
                    add x to left
                for each x in A after or equal to middle
                    add x to right
                left  = merge_sort(left)
                right = merge_sort(right)
                return A = merge(left,right)

        merge(array left,array right):
            declare array result
            while length(left)>0 or length(right)>0
                if length(left)>0 and length(right)>0
                    if left[0]<=right[0]
                        append left[0] to result
                        remove left[0] from left and shift
                    else
                        append right[0] to result
                        remove right[0] from right and shift
                else if length(left)>0
                    append left[0] to result
                    remove left[0] from left and shift
                else if length(right)>0
                    append right[0] to result
                    remove right[0] from right and shift
            return result

- Quick Sort

        input:  array A
        output: sorted array A

        quick_sort(length(A),array A):
            if length(A)<=1
                return A
            else
                pivotIndex = length(A)/2                        // Integer division
                pivot = A[pivotIndex]
                declare arrays left,right of size length(A)-1
                leftCount  = 0
                rightCount = 0
                for i=0 to i=length(A)-1
                    if i=pivotIndex
                        continue
                    else 
                        value = A[i]
                        if A[i]<= pivot
                            left[leftCount] = A[i]
                            leftCount = leftCount+1
                        else
                            right[rightCount] = A[i]
                            rightCount = rightCount+1
                quick_sort(leftCount,left)
                quick_sort(rightCount,right)
                concatenate(left,pivot,right)

- Heap Sort

        input:  array A
        output: sorted array A