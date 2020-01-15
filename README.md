# Task05
1.无重复长度的最长字串
===
```c
int lengthOfLongestSubstring(char * s){
int i,j,k=0,max=0,cnt=0;
for(i=0;s[i]!='\0';i++){
for(j=k;j<i;j++){
if(s[i]==s[j]){
if(cnt>max)max=cnt;
cnt=i-j;
k=j+1;
break;
}
}
if(j==i)cnt++;
if(cnt>max)max=cnt;
}
return max;
}
```

2.串联所有单词的字串
===
```c
int LastFindFlag(int* flagArr, int wordsSize);
int Find(char* compareStr, int* flagArr, char **words, int wordsSize);
#define FALSE -1
#define TRUE 1

int* findSubstring(char * s, char ** words, int wordsSize, int* returnSize){
    *returnSize = 0;
    if (s == NULL || strlen(s) == 0 || words == NULL || wordsSize == 0) {
        return NULL;
    }
    int sLen = strlen(s);
    int wLen = strlen(words[0]);
    int* ret = (int *)malloc(sizeof(int) * sLen);    
    memset(ret, 0, sizeof(int) * sLen);
    int* flagArr = (int*)calloc(sizeof(int), wordsSize);
    char* compareStr = (char*)calloc(sizeof(char), wLen + 1);
    int needFinds = sLen - wLen * wordsSize;
    int retFindflag = FALSE;
    for (int i = 0; i <= needFinds; i++) {
        retFindflag = FALSE;
        for (int j = i; j <= sLen - wLen; j += wLen) {
            strncpy(compareStr, s + j, wLen);    
            retFindflag = Find(compareStr, flagArr, words, wordsSize);
            if (retFindflag == FALSE) {
                break;
            }
        }
        if (LastFindFlag(flagArr, wordsSize) == TRUE) {
            ret[*returnSize] = i;
            *returnSize += 1;
        }
        memset(flagArr, 0, sizeof(int) * wordsSize);
    }
    free(flagArr);
    free(compareStr);
    return ret;
}

int Find(char* compareStr, int* flagArr, char **words, int wordsSize)
{
    for (int i = 0; i < wordsSize; i++) {
        if (flagArr[i] == TRUE) {
            continue;
        }     
        if (strcmp(compareStr, words[i]) == 0) {
            flagArr[i] = TRUE;
            return TRUE;
        }
    }
    return FALSE;
}

int LastFindFlag(int* flagArr, int wordsSize) {
     for (int i = 0; i < wordsSize; i++) {
         if (flagArr[i] != TRUE) {
             return FALSE;
         }
     }
     return TRUE;
}
```

3.替换子串得到平衡字符串
===
```c
#define MIN(a, b) (a) <= (b) ? (a) : (b)
int GetCharId(char c)
{
    switch (c) {
        case 'Q':
            return 0;
        case 'W':
            return 1;
        case 'E':
            return 2;
        case 'R':
            return 3;
        default:
            return 0;
    }
}

int balancedString(char* s)
{
    int i, len, avg, ans, left = 0, right = 0;
    int nums[4] = {0};

    if (s == NULL) {
        return 0;
    }

    len = strlen(s);
    if (len % 4 != 0) {
        return 0;
    }

    avg = len / 4;
    ans = len;
    for (i = 0; i < len; i++) {
        ++nums[GetCharId(s[i])];
    }

    while (right < len) {
        --nums[GetCharId(s[right])];
        while (left < len && nums[0] <= avg && nums[1] <= avg && nums[2] <= avg && nums[3] <= avg) {
            ans = MIN(ans, right - left + 1);
            ++nums[GetCharId(s[left])];
            ++left;
        }
        ++right;
    }

    return ans;
}
```
