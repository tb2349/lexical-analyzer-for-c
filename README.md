<!DOCTYPE html>
<html>
<head>
	<title>LL(1) Parser</title>
</head>
<body>
	<h1>LL(1) Parser Implementation</h1>
	
	<form>
		<label for="input">Input:</label>
		<input type="text" id="input" name="input"><br><br>
		<button type="button" onclick="parse()">Parse</button>
	</form>
	
	<div id="output"></div>
	
	<script>
		function parse() {
			let input = document.getElementById("input").value;
			let table = {
				'S': {'a': ['S->aBa'], 'b': ['S->bAa']},
				'A': {'a': ['A->a'], 'b': ['A->null']},
				'B': {'a': ['B->b'], 'b': ['B->null']}
			};
			let stack = ['$', 'S'];
			let i = 0;
			let action = "";
			let output = "";
			
			while (stack.length > 0) {
				let top = stack.pop();
				if (top === input[i]) {
					action = "Match " + input[i];
					i++;
				} else if (top in table && input[i] in table[top]) {
					let production = table[top][input[i]][0].split('->')[1];
					stack.push(...production.split('').reverse());
					action = top + "->" + production;
				} else {
					action = "Error";
					break;
				}
				output += action + "<br>";
			}
			
			document.getElementById("output").innerHTML = output;
		}
	</script>
	
</body>
</html>
