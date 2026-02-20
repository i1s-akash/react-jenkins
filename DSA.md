    function firstIndex(nums, target) {
        let i = 0, j = nums.length - 1, ans = -1;

        while (i <= j) {
            const mid = (i + j) >> 1;
            const ele = nums[mid]

            if (ele >= target) {
                j = mid - 1;
            } else {
                i = mid + 1;
            }

            if (ele === target) ans = mid;
        }

        return ans;
    }


    function lastIndex(nums, target) {
        let i = 0, j = nums.length - 1;
        let ans = -1;

        while (i <= j) {
            const mid = Math.floor((i + j) / 2);

            if (nums[mid] <= target) {
                i = mid + 1;
            } else {
                j = mid - 1;
            }

            if (nums[mid] === target) ans = mid;
        }

        return ans;
    }

    return [firstIndex(nums, target), lastIndex(nums, target)]