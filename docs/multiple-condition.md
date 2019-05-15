---
id: multiple-condition
title: Mutiple Condition
sidebar_label: Mutiple Condition
---

For more than (2) two condition of element we must use a function. Example below:

## .HTML
---
```
{{showPostDescription(message)}}
```

## .TS
---
```
showPostDescription(message) {
    let sender = this.isSender(message)
    if (message.messages[message.messages.length - 1].post.type == 'market') {
        return `${sender} shared a product.`
    } else if (message.messages[message.messages.length - 1].post.type == 'book') {
        return `${sender} shared a book.`
    } else {
        return `${sender} shared a post.`
    }
}
```

