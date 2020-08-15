# 递归

### 解题步骤

1. 先画出递归树
2. 代码

### 代码模板

```java
private void recursive(int level, int param) {
    // terminator
    if (level == MAX_LEVEL) {
        // process result
        return;
    }
    
    // process current level logic
    process(level, param);
    
    // drill down
    recursive(level + 1, newParam);
    
    // reverse the current level states
}
```

