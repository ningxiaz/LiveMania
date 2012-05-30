/**
 * @fileoverview Classes for Results Page
 *
 * 
 */

var gtv = gtv || {
  jq: {}
};

/**
 * TemplatePage class holds all the support for the page to work
 * and interact with javascript controls.
 * @constructor
 */
gtv.jq.TemplatePage = function() {
};

/**
 * Creates the main menu control.
 */
gtv.jq.TemplatePage.prototype.makeSideNav = function() {
  var templatePage = this;

  var styles = {
    item: 'menu-option',
    itemDiv: 'menu-option',
    row: 'menu-row',
    chosen: 'menu-option-active',
    normal: 'menu-option-off',
    selected: 'menu-option-highlighted'
  };
  var navItems = [];
  
  navItems.push("Artists");
  navItems.push("Albums");
  navItems.push("Tracks");

  var behaviors = {
    orientation: 'vertical',
    selectOnInit: true
  };

  var sidenavParms = {
    createParams: {
      containerId: 'side-nav-container',
      styles: styles,
      keyController: templatePage.keyController,
      choiceCallback: function(selectedItem) {
        choiceCallback(selectedItem);
      }
    },
    behaviors: behaviors
  };

  templatePage.sideNavControl = new gtv.jq.SideNavControl(sidenavParms);

  var showParams = {
    topParent: $('#mainMenu'),
    contents: {
      items: navItems
    }
  };

  templatePage.sideNavControl.showControl(showParams);

  function choiceCallback(selectedItem) {
    templatePage.makeGrid(selectedItem.data('index'));
  }
};

/**
 * Creates the grid control for the selected menu option.
 * @parm {number} selected menu option index.
 */
gtv.jq.TemplatePage.prototype.makeGrid = function( index) {
  var templatePage = this;

  var gridHolder = templatePage.gridHolder;
  if (!gridHolder) {
    gridHolder = $('#grid');
    templatePage.gridHolder = gridHolder;
  } else {
    if (templatePage.gridControl) {
      templatePage.gridControl.deleteControl();
    }
  }

  var styleClasses = {
    page: 'grid-page',
    row: 'grid-row-style',
    itemDiv: 'grid-div',
    item: '',
    description: 'grid-item-description',
    chosen: 'grid-item-active',
    normal: 'grid-item-off',
    selected: 'grid-item-highlighted'
  };
  
  var items = null;
  if(index == 0){
	  items = templatePage.data.artists;
  }
  else if(index == 1){
	  items = templatePage.data.albums;  
  }
  else if(index == 2){
	  items = templatePage.data.tracks;
  }
  
  if (!items || items.length == 0) {
    return;
  }

  var pageItems = [];

  for (var i=0; i<items.length; i++) {
	var item = items[i];

    if(item.image){
    	var img = $('<img></img>')
        	.attr('src', item.image)
        	.addClass('grid-photo');
    }
    else{
    	var img = $('<img></img>')
    		.attr('src', 'images/placeholder.jpg')
    		.addClass('grid-photo');
    }

    var descDiv = $('<div></div>').addClass('slider-text-desc');
    descDiv.append($('<h1></h1>').append(item.name));

    if (item.artist) {
        descDiv.append($('<p></p>').append(item.artist));
    }

    if(item.artist){
    	pageItems.push({ content: img, description: descDiv, data:[item.name, item.artist]});
    }
    else{
    	pageItems.push({ content: img, description: descDiv, data:[item.name]});
    }
    
  }

  var behaviors = {
    itemsPerRow: 3,
    rowsPerPage: 10
  };

  var gridParms = {
    createParams: {
      containerId: 'grid-control-container',
      styles: styleClasses,
      keyController: templatePage.keyController,
      choiceCallback: function(selectedItem) {
        choiceCallback(selectedItem);
      }
    },
    behaviors: behaviors
  };

  templatePage.gridControl = new gtv.jq.GridControl(gridParms);

  var showParams = {
    topParent: templatePage.gridHolder,
    items: pageItems
  };

  templatePage.gridControl.showControl(showParams);

  function choiceCallback(selectedItem) {
    var data = selectedItem.data('nav-data');
    location.assign('fullscreen.html?category=' + data[0] + '&item=' + data[1]);
    if(index == 0){
    	location.assign('fullscreen.html?artist=' + data[0]);
    }
    else if(index == 1){
    	location.assign('fullscreen.html?album=' + data[0] + '&artist=' + data[1]);
    }
    else if(index == 2) {
    	location.assign('fullscreen.html?track=' + data[0] + '&artist=' + data[1]);
    }
  }
};

/**
 * Zooms the page to fit the screen.
 */
gtv.jq.TemplatePage.prototype.doPageZoom = function() {
  var templatePage = this;

  $(document.body).css('zoom', $(window).width()/1205);
};

gtv.jq.TemplatePage.prototype.getKeyword = function(){
	var templatePage = this;
	var queryString = location.search;
	var keyword = queryString.substring(9);
	
	templatePage.keyword = keyword;
}

gtv.jq.TemplatePage.prototype.getData = function(){
	var templatePage = this;
	var keyword = templatePage.keyword;
	templatePage.data = {
		    artists: [],
		    albums: [],
		    tracks: []
		  };
		  
		  $.getJSON("http://ws.audioscrobbler.com/2.0/?method=artist.search&api_key=d8d917df3a6da85d05e177cc9e5299a1&format=json&artist=" + keyword + "&callback=?", function(json) {
				if (json.length!=0){
					//console.log(json);
					//console.log(json.results.artistmatches.artist.length);
					for(var i=0; i<json.results.artistmatches.artist.length; i++){
						var artist = {
								name: json.results.artistmatches.artist[i].name,
								image: json.results.artistmatches.artist[i].image[2]['#text']
						};
						templatePage.data.artists.push(artist);
					}
					templatePage.makeGrid(0);
				}
		  });
		  
		  $.getJSON("http://ws.audioscrobbler.com/2.0/?method=album.search&api_key=d8d917df3a6da85d05e177cc9e5299a1&format=json&album=" + keyword + "&callback=?", function(json) {
				if (json.length!=0){
					//console.log(json);
					//console.log(json.results.albummatches.album.length);
					for(var i=0; i<json.results.albummatches.album.length; i++){
						var album = {
								name: json.results.albummatches.album[i].name,
								artist: json.results.albummatches.album[i].artist,
								image: json.results.albummatches.album[i].image[2]['#text']
						};
						templatePage.data.albums.push(album);
					}
				}
		  });
		  
		  $.getJSON("http://ws.audioscrobbler.com/2.0/?method=track.search&api_key=d8d917df3a6da85d05e177cc9e5299a1&format=json&track=" + keyword + "&callback=?", function(json) {
				if (json.length!=0){
					//console.log(json);
					//console.log(json.results.trackmatches.track.length);
					for(var i=0; i<json.results.trackmatches.track.length; i++){
						var track = {
								name: json.results.trackmatches.track[i].name,
								artist: json.results.trackmatches.track[i].artist
						};
						templatePage.data.tracks.push(track);
					}
				}
		  });
}

/**
 * Starts the template page.
 */
gtv.jq.TemplatePage.prototype.start = function() {
  var templatePage = this;

  templatePage.keyController = new gtv.jq.KeyController();

  templatePage.doPageZoom();
  
  templatePage.getKeyword();

  templatePage.getData();

  templatePage.makeSideNav();

  $(document.body).css('visibility', '');

  templatePage.keyController.start(null, true);
};

new gtv.jq.TemplatePage().start();
