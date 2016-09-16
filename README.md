<html lang="en" >
   <head>
      <link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/angular_material/1.0.0/angular-material.min.css">
      <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular.min.js"></script>
	  <script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.3.15/angular.min.js"></script>
      <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular-animate.min.js"></script>
      <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular-aria.min.js"></script>
      <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular-messages.min.js"></script>
      <script src="https://ajax.googleapis.com/ajax/libs/angular_material/1.0.0/angular-material.min.js"></script>
	  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
	  <style>
	   .box {         
		  color:white;
		  padding:10px;
		  
		  text-align:center;
		  
	   }
	   .green {
	       background:#009688;
		   width:50%;
		   
		   
               
	   }
	   .blue {
	       background:#9c27b0;
		   width:50%;
		  
		   
		}
	   .header{
		   background:gray;
		   text-align:center;
	   }
	   #scroller {
			
			
			overflow: auto;
			
}
	  </style>
      <script language="javascript">
          var app=angular.module('firstApplication', ['ngMaterial']);
            
			 var config = { headers: {
            "authorization": "Bearer 155c68e0e647453d1dc81c3351549ddbba8fad31cbcb719869d502a0d69d17d7",
            
        }
    };
          app.controller ('layoutController', function($scope, $http) {
		  
    
	$scope.spaces = [
        {value : "8ox2awauu9jv", space : "Dev"},
        {value : "5dbvnk7okbss", space : "Stage"},
        {value : "8ox2awauu9jv", space : "ProdSupp"}
    ];
	   $scope.selectedfiles = {};
	$scope.changedValue=function(item){
    $scope.space=item.value;
	$http.get("https://api.contentful.com/spaces/"+item.value+"/locales",config)
	.success(function(response){$scope.locales=response.items;
	});
	$http.get("https://api.contentful.com/spaces/"+item.value+"/public/assets",config)
    .success(function(response) {$scope.names = response.items;
	});
    } //end of changedvalue   
  });
      </script>      
   </head>
   <body ng-app="firstApplication"> 
      <div id="layoutContainer" ng-controller="layoutController as ctrl" ng-cloak>
	  <div>
	  <h1 class="header">Content migration </h1>
	 <div style="text-align: center;">
		<button type="button" onclick="">Migrate</button>
	</div>
	 <br/><br/>
	  </div>
         <div layout="row" layout-xs="column" id ="scroller">
		<!--left pane -->
            <div flex class="green box">
				<label for="selectspace"> Select Space: </label>
				<select name="selectspace" ng-model="selectedSpace" ng-options="x.space for x in spaces" ng-change="changedValue(selectedSpace)">
				<br>
				</select>  
				<label for="selectlocale"> Select Locale: </label>
				<select name="selectlocale" ng-model="selectedLocale" ng-options="x.code for x in locales" ng-change="changedValue(selectedLocale)">
      			</select>
				<h2>Assets </h2>
				{{selectedSpace.value}}
		<!--Fetch all published assets and keep it in selectedfiles variable -->
	 			<ul  ="apicontroller" ng-repeat="x in names">
					<li ng-repeat="(key,value) in x.fields.file">
					{{ value.fileName}}
					<input type="checkbox" name="{{value.fileName}}" id="{{key}}" ng-model="selectedfiles[value.fileName]" >{{key}} &nbsp;&nbsp;{{value.url}}<hr>
					</li>
				</ul >
		
	  </div>
	  <!--right pane -->
            <div flex class="blue box">

				<div ng-repeat="(key,value) in selectedfiles">
			    {{(value==true ? key :null)}}
				</div>
			</div>
			
         
       
      </div>
	  
   </body>
</html>
