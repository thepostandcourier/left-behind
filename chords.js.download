/*//////////////////////////////////////////////////////////
////////////////// Set up the Data /////////////////////////
//////////////////////////////////////////////////////////*/

var NameProvider = ["James Island Charter High","Wando High","West Ashley High","Burke High","St. John’s High","Baptist Hill High","Lincoln High","North Charleston High","Stall High","Academic Magnet High","Chas. County School of the Arts","Chas. Charter for Math and Science","Garrett Academy of Tech","Military Magnet Academy","Pattison’s Academy","Greg Mathis Charter"];
	
var matrix = [
  [998,1,6,1,2,0,0,0,0,90,63,16,5,2,0,0],
  [5,3751,0,1,0,0,0,0,0,305,186,24,9,4,2,0],
  [195,7,1342,17,3,2,0,0,2,114,211,111,64,18,5,6],
  [43,4,18,320,1,0,0,5,4,36,36,52,42,17,2,6],
  [222,1,33,0,281,0,0,0,0,27,23,15,10,3,2,0],
  [28,4,82,0,4,469,0,0,0,13,24,13,31,24,1,0],
  [1,42,0,0,0,0,158,0,0,0,9,0,2,2,1,0],
  [19,9,68,50,0,0,0,439,38,15,34,40,294,96,0,39],
  [47,35,115,23,0,0,0,13,1122,16,61,35,228,79,1,15],
  [90,305,114,36,27,13,0,15,16,0,0,0,0,0,0,0],
  [63,186,211,36,23,24,9,34,61,0,0,0,0,0,0,0],
  [16,24,111,52,15,13,0,40,35,0,0,0,0,0,0,0],
  [5,9,64,42,10,31,2,294,228,0,0,0,0,0,0,0],
  [2,4,18,17,3,24,2,96,79,0,0,0,0,0,0,0],
  [0,2,5,2,2,1,1,0,1,0,0,0,0,0,0,0],
  [0,0,6,6,0,0,0,39,15,0,0,0,0,0,0,0]
];
/*Sums up to exactly 100*/

var colors = ["#21ba45","#f2711c","#e03997","#6435c9","#2185d0","#a333c8","#dc2828","#b5cc18","#00b5ad","#747474","#747474","#747474","#747474","#747474","#747474","#747474"];

/*Initiate the color scale*/
var fill = d3.scale.ordinal()
    .domain(d3.range(NameProvider.length))
    .range(colors);
	
/*//////////////////////////////////////////////////////////
/////////////// Initiate Chord Diagram /////////////////////
//////////////////////////////////////////////////////////*/

var width = 850,
    height = 800,
    hPad = 20
    innerRadius = Math.min(width, height) * .29,
    outerRadius = innerRadius * 1.04;

/*Initiate the SVG*/
var svg = d3.select("#chart").append("svg:svg")
    .attr("viewBox", "0 0 " + width + " " + height)
    .attr("preserveAspectRatio", "xMinYMin meet")
    .append("svg:g")
    .attr("transform", "translate(" + width / 2 + "," + (height / 2 + hPad) + ")");

	
var chord = d3.layout.chord()
    .padding(.04)
    .sortSubgroups(d3.descending) /*sort the chords inside an arc from high to low*/
    .sortChords(d3.descending) /*which chord should be shown on top when chords cross. Now the biggest chord is at the bottom*/
	.matrix(matrix);
	

/*//////////////////////////////////////////////////////////
////////////////// Draw outer Arcs /////////////////////////
//////////////////////////////////////////////////////////*/

var arc = d3.svg.arc()
    .innerRadius(innerRadius)
    .outerRadius(outerRadius);
	
var g = svg.selectAll("g.group")
	.data(chord.groups)
	.enter().append("svg:g")
	.attr("class", function(d) {return "group " + NameProvider[d.index];});
	
g.append("svg:path")
	  .attr("class", "arc")
	  .style("stroke", function(d) { return fill(d.index); })
	  .style("fill", function(d) { return fill(d.index); })
	  .attr("d", arc)
	  .style("opacity", 0)
	  .transition().duration(1000)
	  .style("opacity", 0.4);

/*//////////////////////////////////////////////////////////
////////////////// Line Breaks /////////////////////////////
//////////////////////////////////////////////////////////*/

var insertLinebreaks = function (d) {
    var el = d3.select(this);
    var words = d.split(' ');
    el.text('');

    for (var i = 0; i < words.length; i++) {
        var tspan = el.append('tspan').text(words[i]);
        if (i > 0)
            tspan.attr('x', 0).attr('dy', '15');
    }
};

	
/*//////////////////////////////////////////////////////////
////////////////// Initiate Names //////////////////////////
//////////////////////////////////////////////////////////*/

g.append("svg:text")
  .each(function(d) { d.angle = (d.startAngle + d.endAngle) / 2; })
  .attr("dy", ".45em")
  .attr("class", "titles")
  .attr("text-anchor", function(d) { return d.angle > Math.PI ? "end" : null; })
  .attr("transform", function(d) {
		return "rotate(" + (d.angle * 180 / Math.PI - 90) + ")"
		+ "translate(" + (innerRadius + 25) + ")"
		+ (d.angle > Math.PI ? "rotate(180)" : "");
  })
  .attr('opacity', 0)
  .text(function(d,i) { return NameProvider[i]; });  

/*//////////////////////////////////////////////////////////
//////////////// Initiate inner chords /////////////////////
//////////////////////////////////////////////////////////*/

var chords = svg.selectAll("path.chord")
	.data(chord.chords)
	.enter().append("svg:path")
	.attr("class", "chord")
	.style("stroke", function(d) { return d3.rgb(fill(d.source.index)).darker(); })
	.style("fill", function(d) { return fill(d.source.index); })
	.attr("d", d3.svg.chord().radius(innerRadius))
	.attr('opacity', 0);

/*//////////////////////////////////////////////////////////	
///////////// Initiate Progress Bar ////////////////////////
//////////////////////////////////////////////////////////*/
/*Initiate variables for bar*/
var progressColor = ["#D1D1D1","#949494"],
	progressClass = ["prgsBehind","prgsFront"],
	prgsWidth = 0.4*650,
	prgsHeight = 3;
/*Create SVG to visualize bar in*/
var progressBar = d3.select("#progress").append("svg")
	.attr("width", prgsWidth)
	.attr("height", 3*prgsHeight);
/*Create two bars of which one has a width of zero*/
progressBar.selectAll("rect")
	.data([prgsWidth, 0])
	.enter()
	.append("rect")
	.attr("class", function(d,i) {return progressClass[i];})
	.attr("x", 0)
	.attr("y", 0)
	.attr("width", function (d) {return d;})
	.attr("height", prgsHeight)
	.attr("fill", function(d,i) {return progressColor[i];});

/*//////////////////////////////////////////////////////////	
/////////// Initiate the Center Texts //////////////////////
//////////////////////////////////////////////////////////*/
/*Create wrapper for center text*/
var textCenter = svg.append("g")
					.attr("class", "explanationWrapper");

/*Starting text middle top*/
var middleTextTop = textCenter.append("text")
	.attr("class", "explanation")
	.attr("text-anchor", "middle")
	.attr("x", 0 + "px")
	.attr("y", -24*6/2 + "px")
	.attr("dy", "1em")
	.attr("opacity", 1)
	.style("font-size", "24px")
	.text("How is school choice affecting school populations?")
	.call(wrap, 350);

/*Starting text middle bottom*/
var middleTextBottom = textCenter.append("text")
	.attr("class", "explanation")
	.attr("text-anchor", "middle")
	.attr("x", 0 + "px")
	.attr("y", 24*3/2 + "px")
	.attr("dy", "0.8em")
	.attr('opacity', 1)
	.style("font-size", "18px")
	.text("In a place like Charleston County, the effects can be significant. Take a tour through the numbers")
	.call(wrap, 350);


/*//////////////////////////////////////////////////////////
//////////////// Storyboarding Steps ///////////////////////
//////////////////////////////////////////////////////////*/

var counter = 1,
	buttonTexts = ["Ok","Go on","Next","Ok","Go on","Next","Next","Next",
				   "Next","Next","Next","Next","Next","Finish"],
	opacityValueBase = 0.8,
	opacityValue = 0.4;

/*Reload page*/
d3.select("#reset")
	.on("click", function(e) {location.reload();});

/*Skip to final visual right away*/
d3.select("#skip")
	.on("click", finalChord);
	
/*Order of steps when clicking button*/
d3.select("#clicker")      
	.on("click", function(e){
	
		if(counter == 1) Draw1();
		else if(counter == 2) Draw2();
		else if(counter == 3) Draw3();
		else if(counter == 4) Draw4();
		else if(counter == 5) Draw5();
		else if(counter == 6) Draw6();
		else if(counter == 7) Draw7();
		else if(counter == 8) Draw8();
		else if(counter == 9) Draw9();
		else if(counter == 10) Draw10();
		else if(counter == 11) Draw11();
		else if(counter == 12) Draw12();
		else if(counter == 13) Draw13();
		else if(counter == 14) finalChord();
		
		counter = counter + 1;
	});

/*//////////////////////////////////////////////////////////	
//Introduction
///////////////////////////////////////////////////////////*/
function Draw1(){

	/*First disable click event on clicker button*/
	stopClicker();
		
	/*Show and run the progressBar*/
	runProgressBar(time=700*8);
	
		
	changeTopText(newText = "This chart shows where students live versus where they choose to go to school",
	loc = 4/2, delayDisappear = 0, delayAppear = 1);

	changeTopText(newText = "The full chart is a little complex",
	loc = 6/2, delayDisappear = 7, delayAppear = 8, finalText = true);
	
	changeBottomText(newText = "Let's start at the beginning...",
	loc = 1/2, delayDisappear = 0, delayAppear = 8);
	
	//Remove arcs again
	d3.selectAll(".arc")
		.transition().delay(8*700).duration(2100)
		.style("opacity", 0)
		.each("end", function() {d3.selectAll(".arc").remove();});
		
};/*Draw1*/

/*//////////////////////////////////////////////////////////	
//Show Arc of Apple
//////////////////////////////////////////////////////////*/
function Draw2(){ 

	/*First disable click event on clicker button*/
	stopClicker();
	
	/*Show and run the progressBar*/
	runProgressBar(time=700*2);
	
	var arcDelay = [0,2];
				
	g.append("svg:path")
	  .style("stroke", function(d) { return fill(d.index); })
	  .style("fill", function(d) { return fill(d.index); })
	  .transition().delay(function(d, i) { return 700*arcDelay[i];}).duration(1000)
	  .attr("d", arc)
	  .attrTween("d", function(d) {
		if(d.index == 0 || d.index == 1) {
		   var i = d3.interpolate(d.startAngle, d.endAngle);
		   return function(t) {
			   d.endAngle = i(t);
			 return arc(d);
		   }
		}
	  });

  svg.selectAll("g.group")
	.transition().delay(function(d,i) { return 700*arcDelay[i]; }).duration(700)
	.selectAll("text").style("opacity", 1);
	  
	changeTopText(newText = "Each line represents one school",
	loc = 8/2, delayDisappear = 0, delayAppear = 1, finalText = true);
	
	changeBottomText(newText = "The longer the line, the more students are zoned to attend it",
	loc = 0/2, delayDisappear = 0, delayAppear = 3)	;
	
};/*Draw2*/

/*///////////////////////////////////////////////////////////  
//Draw the other arcs as well
//////////////////////////////////////////////////////////*/
function Draw3(){

	/*First disable click event on clicker button*/
	stopClicker();

	var arcDelay = [0,1,2,3,4,5,6,7,8,10,11,12,13,14,15,16];
	/*Show and run the progressBar*/
	runProgressBar(time=700*20);	
		
   svg.selectAll("g.group").select("path")
	.transition().delay(function(d, i) { return 700*arcDelay[i];}).duration(1000)
	.attrTween("d", function(d) {
		if(d.index > 1) {
		   var i = d3.interpolate(d.startAngle, d.endAngle);
		   console.log(d.startAngle + ", " + d.endAngle);
		   return function(t) {
			   d.endAngle = i(t);
			 return arc(d);
		   }
		}
    });

  svg.selectAll("g.group")
	.transition().delay(function(d,i) { return 700*arcDelay[i]; }).duration(700)
	.selectAll("text").style("opacity", 1);

	changeTopText(newText = "James Island Charter High is the only one that is both a countywide charter and neighborhood school",
		loc = 4/2, delayDisappear = 0, delayAppear = arcDelay[1]);
	changeTopText(newText = "Other charter and magnet schools are shown in grey",
		loc = 7/2, delayDisappear = (arcDelay[9]-1), delayAppear = arcDelay[9], finalText = true);					
	
	changeBottomText(newText = "Students from all over the county can attend these schools",
		loc = -1/8, delayDisappear = 0, delayAppear = (arcDelay[9]+1), w=200);

};/*Draw3*/

/*///////////////////////////////////////////////////////////
//Show the chord between Samsung and Nokia
//////////////////////////////////////////////////////////*/
function Draw4(){

	/*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*6);	
	
	changeTopText(newText = "This is a chord",
		loc = 2/2, delayDisappear = 0, delayAppear = 1, finalText = true, xloc=80, w=280);
	changeTopText(newText = "Each chord represents students zoned for a school but who attend another",
		loc = 5, delayDisappear = 4, delayAppear = 5, finalText = true);
		
	changeBottomText(newText = "Thicker chords represent more students",
		loc = 0, delayDisappear = 0, delayAppear = 6);	
	
    svg.selectAll("g.group").select("path")
		.transition().duration(1000)
		.style("opacity", function(d) {
			if(d.index != 7 && d.index != 12) {return opacityValue;}
		});		
	
	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".titles").style("opacity", function(d) { if(d.index == 7 || d.index == 12) {return 1;} else {return opacityValue;}});

	chords.transition().duration(2000)
		.attr("opacity", function(d, i) { 
			if(d.source.index == 7 && d.target.index == 12) 
				{return opacityValueBase;}
			else {return 0;}
		});
	
};/*Draw4*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw5(){

	/*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*2);
	
	setTimeout(function() {middleTextTop.style("font-size","20px")}, 800);
	
	changeTopText(newText = "In 2014-2015, 294 students from North Charleston High attended Garrett Academy instead",
		loc = 4/2, delayDisappear = 0, delayAppear = 1, finalText = true, xloc=80, w=280);
		
	changeBottomText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);	
	
  svg.selectAll("g.group").select("path")
	.transition().duration(1000)
	.style("opacity", opacityValue);		

	var arcNchs = d3.svg.arc()
				.innerRadius(innerRadius)
				.outerRadius(outerRadius)
				.startAngle(3.8760491532155665+0.1593056474)
				.endAngle(4.299966095442963-0.1513056474);
				
	svg.append("path")
		.attr("class","SamsungToNokiaArc")
		.attr("d", arcNchs)
		.attr("fill", colors[7])
		.style('stroke', colors[7]);
		
	repeat();
	
	function repeat() {
		d3.selectAll(".SamsungToNokiaArc")
			.transition().duration(700)
			.attr("fill", colors[7])
			.style('stroke', colors[7])
			.transition().duration(700)
			.attr("fill", colors[4])
			.style('stroke', colors[4])
			.each("end", repeat)
			;
	};
	
};/*Draw5*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw6(){

	/*First disable click event on clicker button*/
	stopClicker();
	runProgressBar(time=700*3);	
	
	changeTopText(newText = "That's 43% of Garrett Academy's student body",
		loc = 4, delayDisappear = 0, delayAppear = 1, finalText = true);
		
	changeBottomText(newText = "And 26% of the students zoned to attend North Charleston High",
		loc = -1/4, delayDisappear = 0, delayAppear = 2);

	var arcGarrett = d3.svg.arc()
				.innerRadius(innerRadius)
				.outerRadius(outerRadius)
				.startAngle(5.747938934129927)
				.endAngle(5.747938934129927 + 0.10688948666);

	svg.append("path")
		.attr("class","nchstogarrett")
		.attr("d", arcGarrett)
		.attr("fill", colors[4])
		.attr("stroke", colors[4]);
		
  repeat();
	
	function repeat() {
		d3.selectAll(".nchstogarrett")
			.transition().duration(700)
			.attr("fill", colors[7])
			.style('stroke', colors[7])
			.transition().duration(700)
			.attr("fill", colors[4])
			.style('stroke', colors[4])
			.each("end", repeat)
			;
	};	
		
};/*Draw6*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw7(){

	/*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*2);
	
	changeTopText(newText = "These segments show how many students attend the school they're zoned for",
		loc = 11/2, delayDisappear = 0, delayAppear = 1, finalText = true, xloc=-80, w=200);
		
  /*Bottom text disappear*/
	changeBottomText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);
		
	/*Remove the arcs*/
	d3.selectAll(".NokiaToSamsungArc")
		.transition().duration(700)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".NokiaToSamsungArc").remove();});

	d3.selectAll(".nchstogarrett")
		.transition().duration(700)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".SamsungToNokiaArc").remove();});
		
	/*Show only the loyal chords*/
	chords.transition().duration(2000)
		.attr("opacity", function(d, i) { 
			if(d.source.index == 0 && d.target.index == 0) {return opacityValueBase;}
			else if(d.source.index == 1 && d.target.index == 1) {return opacityValueBase;}
			else if(d.source.index == 2 && d.target.index == 2) {return opacityValueBase;}
			else if(d.source.index == 3 && d.target.index == 3) {return opacityValueBase;}
			else if(d.source.index == 4 && d.target.index == 4) {return opacityValueBase;}
			else if(d.source.index == 5 && d.target.index == 5) {return opacityValueBase;}
			else if(d.source.index == 6 && d.target.index == 6) {return opacityValueBase;}
			else if(d.source.index == 7 && d.target.index == 7) {return opacityValueBase;}
			else if(d.source.index == 8 && d.target.index == 8) {return opacityValueBase;}
			else {return 0;}
		});

	/*And the Names of each Arc*/	
	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".titles").style("opacity", 1);
				
};/*Draw7*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw8(){

  /*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*1);
	
	changeTopText(newText = "86% of Wando students attend the school they're zoned for",
		loc = 4/2, delayDisappear = 0, delayAppear = 1, finalText = true, xloc=-50, w=300);
		
  /*Bottom text disappear*/
	changeBottomText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);
		
	/*Remove the arcs*/
	d3.selectAll(".NokiaToSamsungArc")
		.transition().duration(2000)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".NokiaToSamsungArc").remove();});

	d3.selectAll(".SamsungToNokiaArc")
		.transition().duration(2000)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".SamsungToNokiaArc").remove();});
		
	/*Show only the loyal chords*/
	chords.transition().duration(1000)
		.attr("opacity", function(d, i) { 
			if(d.source.index == 0 && d.target.index == 0) {return 0.1;}
			else if(d.source.index == 1 && d.target.index == 1) {return opacityValueBase;}
			else if(d.source.index == 2 && d.target.index == 2) {return 0.1;}
			else if(d.source.index == 3 && d.target.index == 3) {return 0.1;}
			else if(d.source.index == 4 && d.target.index == 4) {return 0.1;}
			else if(d.source.index == 5 && d.target.index == 5) {return 0.1;}
			else if(d.source.index == 6 && d.target.index == 6) {return 0.1;}
			else if(d.source.index == 7 && d.target.index == 7) {return 0.1;}
			else if(d.source.index == 8 && d.target.index == 8) {return 0.1;}
			else {return 0;}
		});
				
};/*Draw8*/

/*///////////////////////////////////////////////////////////	
//Show Loyalty hills
//////////////////////////////////////////////////////////*/
function Draw9(){

  /*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*1);	
	
	changeTopText(newText = "compared to 38% of North Charleston students",
		loc = 3/2, delayDisappear = 0, delayAppear = 1, finalText = true, xloc=30, w=300);
		
  /*Bottom text disappear*/
	changeBottomText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);
		
	/*Remove the arcs*/
	d3.selectAll(".NokiaToSamsungArc")
		.transition().duration(2000)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".NokiaToSamsungArc").remove();});

	d3.selectAll(".SamsungToNokiaArc")
		.transition().duration(2000)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".SamsungToNokiaArc").remove();});
		
	/*Show only the loyal chords*/
	chords.transition().duration(1000)
		.attr("opacity", function(d, i) { 
			if(d.source.index == 0 && d.target.index == 0) {return 0.1;}
			else if(d.source.index == 1 && d.target.index == 1) {return 0.1;}
			else if(d.source.index == 2 && d.target.index == 2) {return 0.1;}
			else if(d.source.index == 3 && d.target.index == 3) {return 0.1;}
			else if(d.source.index == 4 && d.target.index == 4) {return 0.1;}
			else if(d.source.index == 5 && d.target.index == 5) {return 0.1;}
			else if(d.source.index == 6 && d.target.index == 6) {return 0.1;}
			else if(d.source.index == 7 && d.target.index == 7) {return opacityValueBase;}
			else if(d.source.index == 8 && d.target.index == 8) {return 0.1;}
			else {return 0;}
		});
				
};/*Draw9*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw10(){

	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*5);	
	
	changeTopText(newText = "Let's add the chords and segments together",
		loc = 5/2, delayDisappear = 0, delayAppear = 1, finalText = false, xloc=80, w=200);
  changeTopText(newText = "This shows where students zoned for North Charleston High actually go",
		loc = 6/2, delayDisappear = 4, delayAppear = 5, finalText = true, xloc=80, w=200);
		
	d3.selectAll(".NokiaLoyalArc")
		.transition().duration(1000)
		.attr("opacity", 0)
		.each("end", function() {d3.selectAll(".NokiaLoyalArc").remove();});
			
	chords.transition().duration(2000)
    .attr("opacity", function(d, i) { 
		if(d.source.index == 7 || d.target.index == 7) {return opacityValueBase;}
		else {return 0;}
	});

	svg.selectAll("g.group").select("path")
		.transition().duration(2000)
		.style("opacity", function(d) {
			if(d.index != 7) {return opacityValue;}
		});	

	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".titles").style("opacity", function(d) { if(d.index == 7) {return 1;} else {return opacityValue;}});

};/*Draw10*/

/*//////////////////////////////////////////////////////////
//Show all chords that are connected to Apple
//////////////////////////////////////////////////////////*/
function Draw11(){

	/*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*6);	
	
	changeTopText(newText = "They mostly go to charter and magnet schools",
		loc = 4/2, delayDisappear = 0, delayAppear = 1, finalText = false, xloc=80, w=200);
  changeTopText(newText = "...like Garrett Academy and Military Magnet Academy",
		loc = 4/2, delayDisappear = 4, delayAppear = 5, finalText = true, xloc=80, w=200);
			
	/*Only show the chords of NCHS*/
	chords.transition().duration(2000)
    .attr("opacity", function(d, i) { 
		if(d.source.index == 7 || d.target.index == 7) {return opacityValueBase;}
		else {return 0;}
	});

	/*Highlight arc of NCHS*/
	svg.selectAll("g.group").select("path")
		.transition().duration(2000)
		.style("opacity", function(d) {
			if(d.index == 7 || d.index == 12 || d.index == 13 ) {return opacityValueBase;}
			else {return opacityValue;}
		});	

	svg.selectAll("g.group")
		.transition().delay(5000).duration(700)
		.selectAll(".titles").attr("opacity", function(d) { if(d.index == 12 || d.index == 7 || d.index == 13) {return 1;} else {return opacityValue;}});
		
	chords
		.transition().duration(5000)
		.attr("opacity",function (d){
			if(d.source.index == 7) {
				if(d.target.index > 8 || d.target.index == 0) {return opacityValueBase;}
				else {return 0;}
			} else {return 0;}
		});

};/*Draw11*/

/*//////////////////////////////////////////////////////////////////////////*/
function Draw12(){

	stopClicker();
	/*Show and run the progressBar*/
	runProgressBar(time=700*5);	
	
	changeTopText(newText = "Now let's look at charter and magnet schools",
		loc = 5/2, delayDisappear = 0, delayAppear = 1, finalText = false);
  changeTopText(newText = "Color represents where their students come from",
		loc = 9/2, delayDisappear = 7, delayAppear = 8, finalText = true, xloc=110, w=200);
		
  changeBottomText(newText = "These schools draw students from across the county",
		loc = 0, delayDisappear = 0, delayAppear = 3);
  changeBottomText(newText = "",
		loc = 0, delayDisappear = 7, delayAppear = 8);
		
  chords
		.transition().duration(0)
		.attr("opacity", 0);
			
	/*Only show the chords of SOA*/
	chords.transition().duration(2000)
    .attr("opacity", function(d, i) { 
		if(d.source.index == 20 || d.target.index == 20) {return opacityValueBase;}
		else {return 0;}
	});
	
	/*Only show the chords of SOA*/
	chords.transition().delay(3500).duration(2000)
    .attr("opacity", function(d, i) { 
		if(d.source.index == 10 || d.target.index == 10) {return opacityValueBase;}
		else {return 0;}
	});

	/*Highlight arc of SOA*/
	svg.selectAll("g.group").select("path")
		.transition().duration(2000)
		.style("opacity", function(d) {
			if(d.index > 0 && d.index < 9) {return opacityValue;}
		});
		
  /*Highlight arc of SOA*/
	svg.selectAll("g.group").select("path")
		.transition().delay(4000).duration(2000)
		.style("opacity", function(d) {
			if(d.index != 10 ) {return opacityValue;}
		});	

	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".titles").style("opacity", function(d) { if(d.index == 10) {return 1;} else {return opacityValue;}});

};/*Draw10*/


function Draw13(){

	/*First disable click event on clicker button*/
	stopClicker();
	/*Show and run the progressBar*/
	/*runProgressBar(time=700*2);*/

	changeTopText(newText = "Now you're ready to see how many students choose to go to schools outside of their communities",
		loc = 8/2, delayDisappear = 0, delayAppear = 1, finalText = true);
		
	changeBottomText(newText = "Finish the tour and hover over a school to highlight it",
		loc = 3/2, delayDisappear = 0, delayAppear = 1);
		
  /*Highlight arc of SOA*/
	svg.selectAll("g.group").select("path")
		.transition().duration(2000)
		.style("opacity", function(d) {
			if(d.index != 20) {return opacityValue;}
		});	
	
	chords.transition().duration(1000)
		.style("opacity", 0.1);

	/*Hide all the text*/
	d3.selectAll("g.group").selectAll("line")
		.transition().duration(700)
		.style("stroke","#DBDBDB");
	/*Same for the %'s*/
	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".tickLabels").style("opacity",0.4);
	/*And the Names of each Arc*/	
	svg.selectAll("g.group")
		.transition().duration(700)
		.selectAll(".titles").style("opacity",0.4);	
		
};/*Draw14*/

/*///////////////////////////////////////////////////////////
//Draw the original Chord diagram
///////////////////////////////////////////////////////////*/
/*Go to the final bit*/
function finalChord() {
	
	/*Remove button*/
	d3.select("#clicker")
		.style("visibility", "hidden");
	d3.select("#skip")
		.style("visibility", "hidden");
	d3.select("#progress")
		.style("visibility", "hidden");
	
	/*Remove texts*/
	changeTopText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);
	changeBottomText(newText = "",
		loc = 0, delayDisappear = 0, delayAppear = 1);			

	/*Create arcs or show them, depending on the point in the visual*/
	if (counter <= 4 ) {
		g.append("svg:path")
		  .style("stroke", function(d) { return fill(d.index); })
		  .style("fill", function(d) { return fill(d.index); })
		  .attr("d", arc)
		  .style("opacity", 0)
		  .transition().duration(1000)
		  .style("opacity", 1);
		  
	} else {
		 /*Make all arc visible*/
		svg.selectAll("g.group").select("path")
			.transition().duration(1000)
			.style("opacity", 1);
	};
	
	/*Make mouse over and out possible*/
	d3.selectAll(".group")
		.on("mouseover", fade(.02))
		.on("mouseout", fade(.80));
		
	/*Show all chords*/
	chords.transition().duration(1000)
		.style("opacity", opacityValueBase);

	/*Show all the text*/
	d3.selectAll("g.group").selectAll("line")
		.transition().duration(100)
		.style("stroke","#000");
	/*Same for the %'s*/
	svg.selectAll("g.group")
		.transition().duration(100)
		.selectAll(".tickLabels").style("opacity",1);
	/*And the Names of each Arc*/	
	svg.selectAll("g.group")
		.transition().duration(100)
		.selectAll(".titles").style("opacity",1);		

};/*finalChord*/

/*//////////////////////////////////////////////////////////
////////////////// Extra Functions /////////////////////////
//////////////////////////////////////////////////////////*/

/*Returns an event handler for fading a given chord group*/
function fade(opacity) {
  return function(d, i) {
    svg.selectAll("path.chord")
        .filter(function(d) { return d.source.index != i && d.target.index != i; })
		.transition()
        .style("stroke-opacity", opacity)
        .style("fill-opacity", opacity);
  };
};/*fade*/

/*Returns an array of tick angles and labels, given a group*/
function groupTicks(d) {
  var k = (d.endAngle - d.startAngle) / d.value;
  return d3.range(0, d.value, 1).map(function(v, i) {
    return {
      angle: v * k + d.startAngle,
      label: i % 5 ? null : v + "%"
    };
  });
};/*groupTicks*/

/*Taken from https://groups.google.com/forum/#!msg/d3-js/WC_7Xi6VV50/j1HK0vIWI-EJ
//Calls a function only after the total transition ends*/
function endall(transition, callback) { 
    var n = 0; 
    transition 
        .each(function() { ++n; }) 
        .each("end", function() { if (!--n) callback.apply(this, arguments); }); 
};/*endall*/ 

/*Taken from http://bl.ocks.org/mbostock/7555321
//Wraps SVG text*/
function wrap(text, width) {
    var text = d3.select(this)[0][0],
        words = text.text().split(/\s+/).reverse(),
        word,
        line = [],
        lineNumber = 0,
        lineHeight = 1.4, 
        y = text.attr("y"),
		x = text.attr("x"),
        dy = parseFloat(text.attr("dy")),
        tspan = text.text(null).append("tspan").attr("x", x).attr("y", y).attr("dy", dy + "em");
		
    while (word = words.pop()) {
      line.push(word);
      tspan.text(line.join(" "));
      if (tspan.node().getComputedTextLength() > width) {
        line.pop();
        tspan.text(line.join(" "));
        line = [word];
        tspan = text.append("tspan").attr("x", x).attr("y", y).attr("dy", ++lineNumber * lineHeight + dy + "em").text(word);
      };
    };  
};

/*Transition the top circle text*/
function changeTopText (newText, loc, delayDisappear, delayAppear, finalText, xloc, w) {

	/*If finalText is not provided, it is not the last text of the Draw step*/
	if(typeof(finalText)==='undefined') finalText = false;
	
	if(typeof(xloc)==='undefined') xloc = 0;
	if(typeof(w)==='undefined') w = 350;
	
	middleTextTop	
		/*Current text disappear*/
		.transition().delay(700 * delayDisappear).duration(700)
		.attr('opacity', 0)	
		/*New text appear*/
		.call(endall,  function() {
			middleTextTop.text(newText)
			.attr("y", -24*loc + "px")
			.attr("x", xloc + "px")
			.call(wrap, w);	
		})
		.transition().delay(700 * delayAppear).duration(700)
		.attr('opacity', 1)
		.call(endall,  function() {
			if (finalText == true) {
				d3.select("#clicker")
					.text(buttonTexts[counter-2])
					.style("pointer-events", "auto")
					.transition().duration(400)
					.style("border-color", "#363636")
					.style("color", "#363636");
				};
		});
};/*changeTopText */

/*Transition the bottom circle text*/
function changeBottomText (newText, loc, delayDisappear, delayAppear) {
	middleTextBottom
		/*Current text disappear*/
		.transition().delay(700 * delayDisappear).duration(700)
		.attr('opacity', 0)
		/*New text appear*/
		.call(endall,  function() {
			middleTextBottom.text(newText)
			.attr("y", 24*loc + "px")
			.call(wrap, 350);	
		})
		.transition().delay(700 * delayAppear).duration(700)
		.attr('opacity', 1);
;}/*changeTopText*/

/*Stop clicker from working*/
function stopClicker() {
	d3.select("#clicker")
		.style("pointer-events", "none")
		.transition().duration(400)
		.style("border-color", "#D3D3D3")
		.style("color", "#D3D3D3");
};/*stopClicker*/

/*Run the progress bar during an animation*/
function runProgressBar(time) {	
	
	/*Make the progress div visible*/
	d3.selectAll("#progress")
		.style("visibility", "visible");
	
	/*Linearly increase the width of the bar
	//After it is done, hide div again*/
	d3.selectAll(".prgsFront")
		.transition().duration(time).ease("linear")
		.attr("width", prgsWidth)
		.call(endall,  function() {
			d3.selectAll("#progress")
				.style("visibility", "hidden");
		});
	
	/*Reset to zero width*/
	d3.selectAll(".prgsFront")
		.attr("width", 0);
		
};/*runProgressBar*/