# Notes
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
