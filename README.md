/*I added some more staff like the pages and I added the caracters */
fill(0, 0, 0);
textSize(20);
textAlign(CENTER);


var currentScene;

var drawScene1 =function(){
    currentScene = 1;
background(255, 255, 255);
fill(255, 0, 0);
text("欢迎来到游戏", 159, 215);
};

var drawScene2 =function(){
    currentScene = 2;
 background(255, 255, 255);
fill(255, 0, 0); 
text("单击任意位置并开始播放", 159, 215);
};

var drawScene3 =function(){
    currentScene = 3;
  var Tile = function(x, y, face) {
    this.x = x;
    this.y = y;
    this.size = 70;
    this.face = face;
    this.isFaceUp = false;
    this.isMatch = false;
};

Tile.prototype.draw = function() {
    fill(214, 247, 202);
    strokeWeight(2);
    rect(this.x, this.y, this.size, this.size, 10);
    if (this.isFaceUp) {
        image(this.face, this.x, this.y, this.size, this.size);
    } else {
        image(getImage("avatars/leaf-green"), this.x, this.y, this.size, this.size);
    }
};

Tile.prototype.isUnderMouse = function(x, y) {
    return x >= this.x && x <= this.x + this.size  &&
        y >= this.y && y <= this.y + this.size;
};

// Global config
var NUM_COLS = 5;
var NUM_ROWS = 4;

// Declare an array of all possible faces
var faces = [
    getImage("cute/CharacterBoy"),
    getImage("cute/CharacterCatGirl"),
    getImage("cute/CharacterHornGirl"),
    getImage("cute/CharacterPinkGirl"),
    getImage("cute/CharacterPrincessGirl"),
    getImage("cute/ChestClosed"),
    getImage("cute/ChestLid"),
    getImage("cute/DoorTallClosed"),
    getImage("cute/DoorTallOpen"),
    getImage("avatars/robot_female_2")
];

// Make an array which has 2 of each, then randomize it
var possibleFaces = faces.slice(0);
var selected = [];
for (var i = 0; i < (NUM_COLS * NUM_ROWS) / 2; i++) {
    // Randomly pick one from the array of remaining faces
    var randomInd = floor(random(possibleFaces.length));
    var face = possibleFaces[randomInd];
    // Push twice onto array
    selected.push(face);
    selected.push(face);
    // Remove from array
    possibleFaces.splice(randomInd, 1);
}

// Now shuffle the elements of that array
var shuffleArray = function(array) {
    var counter = array.length;

    // While there are elements in the array
    while (counter > 0) {
        // Pick a random index
        var ind = Math.floor(Math.random() * counter);
        // Decrease counter by 1
        counter--;
        // And swap the last element with it
        var temp = array[counter];
        array[counter] = array[ind];
        array[ind] = temp;
    }
};
shuffleArray(selected);

// Create the tiles
var tiles = [];
for (var i = 0; i < NUM_COLS; i++) {
    for (var j = 0; j < NUM_ROWS; j++) {
        var tileX = i * 78 + 10;
        var tileY = j * 78 + 40;
        var tileFace = selected.pop();
        tiles.push(new Tile(tileX, tileY, tileFace));
    }
}

background(255, 255, 255);

var numTries = 0;
var numMatches = 0;
var flippedTiles = [];
var delayStartFC = null;

mouseClicked = function() {
    for (var i = 0; i < tiles.length; i++) {
        var tile = tiles[i];
        if (tile.isUnderMouse(mouseX, mouseY)) {
            if (flippedTiles.length < 2 && !tile.isFaceUp) {
                tile.isFaceUp = true;
                flippedTiles.push(tile);
                if (flippedTiles.length === 2) {
                    numTries++;
                    if (flippedTiles[0].face === flippedTiles[1].face) {
                        flippedTiles[0].isMatch = true;
                        flippedTiles[1].isMatch = true;
                        flippedTiles.length = 0;
                        numMatches++;
                    }
                    delayStartFC = frameCount;
                }
            } 
            loop();
        }
    }
};

draw = function() {
    background(255, 255, 255);
    if (delayStartFC && (frameCount - delayStartFC) > 30) {
        for (var i = 0; i < tiles.length; i++) {
            var tile = tiles[i];
            if (!tile.isMatch) {
                tile.isFaceUp = false;
            }
        }
        flippedTiles = [];
        delayStartFC = null;
        noLoop();
    }
    
    for (var i = 0; i < tiles.length; i++) {
        tiles[i].draw();
    }
    
    if (numMatches === tiles.length/2) {
        fill(0, 0, 0);
        textSize(20);
        text("You found them all in " + numTries + " tries!", 20, 375);
    }
};

noLoop();  
};

var drawScene4 =function(){
    currentScene = 4;
    background(235, 226, 235);
    fill(255, 0, 0);
    text("你警告游戏",  159, 215);
   
};

var drawScene5 =function(){
    currentScene = 5;
   background(235, 226, 235);
    fill(255, 0, 0);
    text("去喝价格",  159, 215);
};




drawScene1(); mouseClicked=function() {
    if (currentScene === 1) {
        drawScene2();
    } else if (currentScene === 2) {
        drawScene3();
    } else if (currentScene === 3) {
        drawScene4();
    } else if (currentScene === 4) {
        drawScene5();
    } else if (currentScene === 5) {
        drawScene1();  
    }
};

