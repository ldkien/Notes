# Notes

-----------------------------------------------------------
```javascript
$scope.readFile = function(files,success) {
    	var data= new Array();
    	if(files===undefined||files==null){
    		return success(data);
    	}
      //tạo Promise - bất đồng bộ đọc nhiều file
    	return Promise.all([].map.call(files, function (file) {
 	        return new Promise(function (resolve, reject) {
 	            var reader = new FileReader();
 	            reader.onloadend = function () {
 	                resolve({name: file.name, data:reader.result});
 	            };
 	            reader.readAsDataURL(file);
 	        });
 	    })).then(function (results) {	       
 	        return success(results);
 	    });
	}
```
------------------------------------------------------------
# Vòng đời của component
### Khởi tạo Component
Lần lượt các hành động sau để khởi tạo component:

- Khởi tạo Class (đã thừa kế từ class Component của React)
- Khởi tạo giá trị mặc định cho Props (defaultProps)
- Khởi tạo giá trị mặc định cho State (trong hàm constuctor)
- Gọi hàm componentWillMount()
- Gọi hàm render()
- Gọi hàm componentDidMount()
### Khi State thay đổi
- Cập nhật giá trị cho state
- Gọi hàm shouldComponentUpdate()
- Gọi hàm componentWillUpdate() – với điều kiện hàm trên return true
- Gọi hàm render()
- Gọi hàm componentDidUpdate()
- Khi Props thay đổi
### Cập nhật giá trị cho props
- Gọi hàm componentWillReceiveProps()
- Gọi hàm shouldComponentUpdate()
- Gọi hàm componentWillUpdate() – với điều kiện hàm trên return true
- Gọi hàm render()
- Gọi hàm componetDidUpdate()
### Khi Unmount component
- Gọi hàm componentWillUnmount()
---------------------------------------------------------------------
# Những điều nên hay ko nên làm trong life-cycle react
### Contructor
	* Nên:
		> thiết lập state ban đầu
		> nếu không sử dụng cú pháp thuộc tính class -> 
			chuẩn bị tất cả các fields class và ràng buộc 
			các function sẽ được pass như callbacks
	* Ko:
    		> gây ra bất kỳ side-effects (hiệu ứng lề) (gọi AJAX ....)
### componentWillMount
	* Nên:
		> Cập nhật state
	* Ko: 
		> gây ra bất kỳ side-effects (hiệu ứng lề) (gọi AJAX ....)
### componentWillReceiveProps(nextProps)
- Function này sẽ được gọi trong mỗi lần update vòng đời do các thay đổi đối với props
(component cha mẹ thực hiện re-rendering)
- Function này là lý tưởng nếu bạn có một component mà các phần của state 
phụ thuộc vào props được truyền từ component cha mẹ như việc 
gọi this.setState ở đây sẽ không gây ra thêm một lời gọi render
```
componentWillReceiveProps(nextProps) {
  if(nextProps.myProp !== this.props.myProps) {
    // nextProps.myProp có giá trị khác với current prop của chúng ta
    // nên chúng ta có thể thực hiện các phép tính trên giá trị mới
  }
}
```
	* Nên:
		> đồng bộ hóa state với props
	* Ko:
    		> gây ra bất kỳ side-effects (hiệu ứng lề) (gọi AJAX ....)
### shouldComponentUpdate(nextProps, nextState, nextContext)
	* Nên: 
		> quyết định có cập nhật hay không khi trạng thay đổi state, props
	* Ko:
		> gây ra bất kỳ side-effects (hiệu ứng lề) (gọi AJAX ....)
		> gọi setState
### componentWillUpdate(nextProps, nextState)
- Có thể được sử dụng thay cho componentWillReceiveProps trong trường hợp shouldComponentUpdate đc implement(true)
vì khi này component chắc chắn được render lại
	* Nên:
		> đồng bộ hóa state với props
	* Ko:
    		> gây ra bất kỳ side-effects (hiệu ứng lề) (gọi AJAX ....)
### componentDidUpdate(prevProps, prevState, prevContext)
- hàm này được đảm bảo gọi duy nhất trong quá trình re-render
-  hàm này được gọi với các object-maps của các props, state và context **trước đây** => kiểm tra
nếu giá trị đã cho thay đổi->thực hiện các thay đổi tiếp theo:
```
  if(prevProps.myProps !== this.props.myProp) {
    // this.props.myProp có một giá trị khác
    // chúng ta có thể thực thi các operations mà sẽ cần
    // giá trị mới và/hoặc gây ra side-effects 
    // như gọi AJAX với giá trị mới - this.props.myProp
  }
````
	* Nên:
		> tạo ra side-effects (hiệu ứng lề) (gọi AJAX ....)
	* Ko:
		> gọi this.setState vì nó sẽ tạo ra một re-render
**


