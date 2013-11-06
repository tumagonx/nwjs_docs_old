The following changes to DOM is made to support native applications better:

Only `Node frames` can do this. `Normal frames` still follow the W3C standard. For definition of `Node frames` and `Normal frames`, see [Security](Security)

### setting value of file input element
```javascript
var f = new File('/path/to/file', 'name');
var files = new FileList();
files.append(f);
document.getElementById('input0').files = files;
```