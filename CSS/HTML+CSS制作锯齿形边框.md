```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		.header{
		      position:relative;
		      color:white;
		      background-color:#2B3A48;
		      text-align:center;
		}
		.header:after {
		  content: "";
		  position: absolute;      
		  display: block;

		  height: 10px;
		  bottom: -10px; /* -height */
		  left:0;
		  right:0;

		  /* TODO Add browser prefixes */
		  background:
		    linear-gradient(
		      45deg, transparent 33.333%,
		      #2B3A48 33.333%, #2B3A48 66.667%,
		      transparent 66.667%
		    )
		    ,linear-gradient(
		      -45deg, transparent 33.333%,
		      #2B3A48 33.333%, #2B3A48 66.667%,
		      transparent 66.667%
		    );

		    background-size: 8px 20px; /* toothSize doubleHeight */
		    background-position: 0 -10px; /* horizontalOffset -height */
		}
	</style>	
</head>
<body>
	<div class="header"><h1>This is a header</h1></div>
</body>
</html>
```

<image src="./imgs/gradient.jpg" width=600>