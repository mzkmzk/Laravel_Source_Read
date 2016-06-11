# 系统初始化流程

# 总体

```flow
start=>start: Start 
e=>end

start->e
```

```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```

'''mermaid
graph TD;
 A-->B;
 A-->C;
 B-->D;
 C-->D;
'''



