# RPG
var x = 200;
var y = 300;
var direction = 1;
var speed = 2;
var playerSize = 20;
var buildings = [];
var cur_mapID = 0;
//building CLASS/OBJECT!!!!!!
var builiding = function (x,y,width,height,r,g,b,state){
    this.height = height;
    this.width = width;
    this.x = x;
    this.y = y;
    this.r = r;
    this.g = g;
    this.b = b;
    //solid,transparent,action
    this.state = state;
    this.push2list();
    
};

builiding.prototype.push2list = function() {
    buildings.push([this.x,this.y,this.width,this.height,this.state]);
    
};

builiding.prototype.draw = function() {
    fill(this.r,this.g,this.b);
    rect(this.x,this.y,this.width,this.height,10);
    };

/**var returnSide = function(x,y,height,width,whichside){
    if(whichside === "left"){
        return [x,y,x,y-height];
    }
    else if(whichside === "right"){
        return [x+ width,y,x+width,y-height];
    }
    else if(whichside === "bottom"){
        return [x,y-height,x+width,y-height];
    }
    else if(whichside === "up"){
        return [x,y,x+width,y];
    }
    
};
**/
var determine_touching_action = function(playerX,playerY){
    
  
    //playerSide = returnSide(playerX,playerY,playerSize,playerSize,playerSide);
    //println("playerSide");
    //println(playerSide);

    for (var i = 0; i < buildings.length; i++) {
        //buildings[i][0],buildings[i][1],buildings[i][2],buildings[i][3]
        if(x+playerSize/2 >= buildings[i][0] &&
        x+playerSize/2 <= buildings[i][0]+buildings[i][2] && 
        y+playerSize/2 >= buildings[i][1] && 
        y+playerSize/2 <= buildings[i][1]+buildings[i][3]
        ){
            
            if (buildings[i][4] === "solid"){
                return 1;}
            else if (buildings[i][4] === "transparent"){
                return 3;}
            else if (buildings[i][4] === "action"){
                return 2;}
        }
        
           
        
    }
return 3;
};

//player class/object
var player = function(){
    var currentx =x;
    var currenty= y;
    if (keyCode === UP) {
        y = y - speed;
        direction = 1;
        ellipse(x-2,y+2,12,12);
        ellipse(x+22,y+2,12,12);
        }
    if (keyCode === DOWN) {
        y = y + speed;
        ellipse(x-2,y+19,12,12);
        ellipse(x+22,y+19,12,12);
        direction = 2;
        }
    if (keyCode === LEFT) {
        x = x - speed;
        ellipse(x-2,y+19,12,12);
        ellipse(x-2,y+2,12,12);
        direction = 4;
        }
    if (keyCode === RIGHT) {
        x = x + speed;
        ellipse(x+22,y+19,12,12);
        ellipse(x+22,y+2,12,12);
        direction = 3;
        }
    var action = determine_touching_action(x,y,playerSize);
     if (action !== 3){
        x = currentx;
        y = currenty;
     }
};
var playerhitbox = function(){
    strokeWeight(10);
    
    point(x+playerSize/2,y+playerSize/2);
    
};
var outdoor_map = function(){
    background(53, 196, 79);
    //building
    buildings = [];
    var build1 = new builiding(110,100,170,70,143,97,19,"solid");
    build1.draw();
    //door
    var build2 = new builiding(165,170,60,6,140, 117, 70,"action");
    build2.draw();
    println(buildings);
    //roof tiles
    line(120, 106, 273, 160);
    line(120, 164, 269, 102);
};
var indoor_map = function(){
    background(156, 120, 11);
};

var determine_map = function(){
    if (cur_mapID===0){
        outdoor_map();
    }   
};
draw = function() {
    determine_map();
    //player
    //Main player body
    fill(242, 194, 51);
    rect(x,y,playerSize,playerSize,6);
    playerhitbox();
    strokeWeight(3);
    
    
    if(keyIsPressed){
        player();
    }
    
    else {
        if (direction === 1){
            ellipse(x-2,y+2,12,12);
            ellipse(x+22,y+2,12,12);
        }
        else if (direction === 2){
            ellipse(x-2,y+19,12,12);
            ellipse(x+22,y+19,12,12);
        }
        else if (direction === 3){
            ellipse(x+22,y+19,12,12);
            ellipse(x+22,y+2,12,12);
            
        } 
        
        else if (direction === 4){
            ellipse(x-2,y+19,12,12);
            ellipse(x-2,y+2,12,12);
        }
        
    }
    
    //border "teleporter"
    if (x > 400){
        x = 0.9999;
    } 
    if (x < 0){
        x = 399.999;
    } 
    if (y < 0){
        y = 399.999;
    } 
    if (y > 400){
            y = 0.9999;
    } 
   
};
