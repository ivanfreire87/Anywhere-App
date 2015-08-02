var db;
var dbCreated = false;
var internetStatus;
var id = 1;
var chart;
var numAbsences;


$(document).ready(function(){
	app.initialize();
});

var app = {

		initialize: function() {
			this.bindEvents()
		},

		bindEvents: function() {
			document.addEventListener('deviceready', this.onDeviceReady, false)
			$(document).on("pageshow", app.onDeviceReady);
			//$(document).on('pageshow', '#index' , fullcalendar);
			
		},

		onDeviceReady: function() {
			app.receivedEvent('deviceready')
		},
		
		receivedEvent: function(id){
			
			
			
//			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'index.html'){
//				navigator.splashscreen.show();
//				
//				setTimeout(function() {
//			        navigator.splashscreen.hide();
//			    }, 3000);
//			}
			
			//container between header and footer
			$(".container").css("height", "100%").css("height", "-=110")
			
			//balance text align center vertically (dinner)		
			var height = $('#getBalance').height();
			$('#getBalance').css({
				'line-height': height + 'px'
			});			
			
			getNews()
			changeInternetStatus()
			blockNewLinesInTextBoxes()
			
			db = window.openDatabase("AnywhereDB", "1.0", "Anywhere", 200000);
			if (dbCreated){
				db.transaction(getAbsences, transaction_error);
			}	
			else
				db.transaction(populateDB, transaction_error, populateDB_success);



			//Events
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'dinner.html'){
				document.getElementById('getBalance').addEventListener('click', this.getBalance, false)
				var balance = window.localStorage.getItem("balance")
				var updateDate = window.localStorage.getItem("balanceLastUpdate")					
				
				$("#getBalance").text(balance + " €")
				$("#balanceUpdated").text("Last update: " + updateDate)
			}
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendar.html'){
				document.getElementById('addAbsence').addEventListener('click', function() {
					$('#addAbsenceBox').show()
					$('#addAbsenceBoxOuter').show()
				}, false)
				document.getElementById('cancelNewAbsence').addEventListener('click', this.cancelNewAbsence, false)
				document.getElementById('okNewAbsence').addEventListener('click', this.okNewAbsence, false)	
				
				var absenceListHeigth = $(window).height()- 110 - $(window).width()*0.6 - 65
				$('#absencesListContainer').css("height",absenceListHeigth)

				db.transaction(function (tx){
					tx.executeSql("SELECT id,type,absenceFrom FROM absences", [], function (tx, results) {
						var len = results.rows.length;
						var absences =  new Array(len-1)
						
						for (var i=0; i<len; i++) {
							var absence = results.rows.item(i);
							absences[i] = [absence.absenceFrom, absence.type, absence.id]
						}

						$("#calendar").webix_calendar({
							weekHeader:true,
							id:"calendar",
							dayTemplate: function(date){
								var html = "<div class='day'>"+date.getDate()+"</div>";
								for (var i=0; i<absences.length; i++) {
									var absenceDateFrom = absences[i][0]
									var absenceColor = getTypeColor(absences[i][1])
									var yyyyFrom = absenceDateFrom.split("-")[0];           
									var mmFrom = absenceDateFrom.split("-")[1];
									var ddFrom = absenceDateFrom.split("-")[2];
											
									if(date.getDate() == ddFrom && date.getMonth() == mmFrom-1 && date.getFullYear()==yyyyFrom)
										html = "<div align='center' class='day'><div style='background-color:"+ absenceColor +"; color:white; width:25px !important; height:25px !important; box-sizing: border-box; border-radius:6px;'>"+date.getDate()+"</div></div>";							
								}
								return html;
							},
							width:$(window).width(),
							height:$(window).width()*0.6
						});
						
						$$('calendar').attachEvent("onDateselect", function(date){
							var container = $('#absencesListContainer')
							
							for(i=0;i<absences.length;i++){
								var absenceDateFrom = absences[i][0]
								var yyyyFrom = absenceDateFrom.split("-")[0];           
								var mmFrom = absenceDateFrom.split("-")[1];
								var ddFrom = absenceDateFrom.split("-")[2];
								
								if(date.getDate() == ddFrom && date.getMonth() == mmFrom-1 && date.getFullYear()==yyyyFrom){

									var scrollTo = $('#absenceInfoItem' + absences[i][2] + '')

									container.animate({
										scrollTop: scrollTo.offset().top - container.offset().top + container.scrollTop()
									});


									$('#absenceInfoItem' + absences[i][2] + '').click()
								}
							}
						});
					});
				});
				webix.Touch.disable();	
			}
			
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendarStatistics.html'){
				document.getElementById('addFilter').addEventListener('click', function() {
					$('#addFilterBox').show()
					$('#addFilterBoxOuter').show()
				}, false)
				
				document.getElementById('cancelNewFilter').addEventListener('click', this.cancelNewFilter, false)
				document.getElementById('okNewFilter').addEventListener('click', this.okNewFilter, false)	
			}
			
			document.addEventListener('online', this.onChangeConnectivity, false)	
			document.addEventListener('offline', this.onChangeConnectivity, false)
			
			
			//alert(location.pathname.substring(location.pathname.lastIndexOf("/") + 1))

//			var str = navigator.userAgent.toLowerCase()
//			
//			var version = str.match(/android\s+([\d\.]+)/)[1];
			
			
			
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendar.html'){
				//alert(location.pathname.substring(location.pathname.lastIndexOf("/") + 1))
				$('.container').on('swipeleft',this.onCalendarSwipeLeft);
				$("#next").show()
				$("#next").on('click',this.onCalendarSwipeLeft);
				$("#prev").hide()
			}
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendarAbsencesToApprove.html'){
				//alert(location.pathname.substring(location.pathname.lastIndexOf("/") + 1))			
				$('.container').on('swipeleft',this.onCalendarAbsencesToApproveSwipeLeft);
				$("#next").show()
				$("#next").on('click',this.onCalendarAbsencesToApproveSwipeLeft);
				var absenceToApproveListHeigth = $(window).height()- 110 - 55   // - (header+footer) - prev/next 
				$('#absencesToApproveListContainer').css("height",absenceToApproveListHeigth)
				
			}
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendarAbsencesToApprove.html'){
				//alert(location.pathname.substring(location.pathname.lastIndexOf("/") + 1))
				$('.container').on('swiperight',this.onCalendarAbsencesToApproveSwipeRight);
				$("#prev").show()
				$("#prev").on('click',this.onCalendarAbsencesToApproveSwipeRight);
			}
			if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendarStatistics.html'){	
				//alert(location.pathname.substring(location.pathname.lastIndexOf("/") + 1))
				$('.container').on('swiperight',this.onCalendarAbsencesStatisticsSwipeRight);	
				drawChart("SELECT DISTINCT type,absenceFrom,absenceTo,approved, Count(type) AS typeCount FROM absences GROUP BY type");
				$("#prev").show()
				$("#prev").on('click',this.onCalendarAbsencesStatisticsSwipeRight);
				$("#next").hide()
				
			}

		},
		
		cancelNewAbsence: function (){
		    $('#addAbsenceBox').hide()
		    $('#addAbsenceBoxOuter').hide()
		},
		
		cancelNewFilter: function (){
		    $('#addFilterBox').hide()
		    $('#addFilterBoxOuter').hide()
		},
		
		okNewAbsence: function (){
						
			var absenceType = document.getElementById('absenceType').value
			var absenceText = document.getElementById('noteNewAbsence').value
			var absenceDateFrom = document.getElementById('absenceFrom').value
			var absenceDateTo = document.getElementById('absenceTo').value
						
			db.transaction(
					function(tx) {        
						tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" 
								+ id++  				    		                                                                                          
								+ ",'" 				    		                                                                                        
								+ String(absenceType)				    		
								+ "','"				    		
								+ String(absenceDateFrom)					    		
								+ "','" 					    		
								+ String(absenceDateTo)					    		
								+ "','" 					    		
								+ absenceText					    		
								+ "'," 					    		
								+ "'pending'"  
								+ "," 					    		
								+ "'images/julie_taylor.jpg'"   //url da foto da pessoa que cria a ausencia 		 			    		
								+ ")");
					}, transaction_error);
			
			db.transaction(getAbsences, transaction_error);	
			$('#absencesList').show()			
			$('#addAbsenceBox').hide()
		    $('#addAbsenceBoxOuter').hide()
		    
		    document.getElementById('absenceType').selectedIndex = 0
		    document.getElementById('noteNewAbsence').value = ""
		    document.getElementById('absenceFrom').value = ""
		    document.getElementById('absenceTo').value = ""		    
		},
		
		okNewFilter: function (){
			
			var absenceType = $('#filterAbsenceType :selected')
			var absenceDateFrom = document.getElementById('filterAbsenceFrom').value
			var absenceDateTo = document.getElementById('filterAbsenceTo').value
			var approved = document.getElementById('filterApproved').value
			var pending = document.getElementById('filterPending').value
			var rejected = document.getElementById('filterRejected').value
			var absencesToFilter = "";
			var states = "";
			 
			
						
			if(absenceDateFrom=='')
				absenceDateFrom='0000-00-00';
			if(absenceDateTo=='')
				absenceDateTo='9999-99-99';
			
			for(i=0; i<absenceType.length; i++){
				absencesToFilter += "type in ('" + String(absenceType[0].value) + "' "
				for(i=1; i<absenceType.length-1; i++){
					absencesToFilter += ",'" + String(absenceType[i].value) + "' "
				}
				absencesToFilter += ",'" + String(absenceType[absenceType.length-1].value) + "') AND "
			}
			
			
			if(approved == 'on')
				states += "AND approved in ('true',";
			else
				states += "AND approved in ('',"	;

			if(pending == 'on')
				states += "'pending',";
			else
				states += "'',"	;
			
			if(rejected == 'on')
				states += "'false')";
			else
				states += "'')"	;
				
			
			sql="SELECT DISTINCT type,absenceFrom,absenceTo,approved, Count(type) AS typeCount FROM absences WHERE " 
			+ absencesToFilter
			+ "absenceFrom >= '" + String(absenceDateFrom)
			+ "' "
			+ "AND absenceTo <= '" + String(absenceDateTo) + "' "
			+ states
			+ " GROUP BY type";				
		
			$('#addFilterBox').hide()
		    $('#addFilterBoxOuter').hide()
			
		   
			drawChart(sql);
	    
		},
		
		onCalendarSwipeLeft: function(){
			app.onSwipe('calendar','left')
		},
		
		onCalendarAbsencesToApproveSwipeLeft: function(){
			app.onSwipe('calendarAbsencesToApprove','left')
		},
		
		onCalendarAbsencesToApproveSwipeRight: function(){
			app.onSwipe('calendarAbsencesToApprove','right')
		},
		
		onCalendarAbsencesStatisticsSwipeRight: function(){
			app.onSwipe('calendarAbsencesStatistics','right')
		},

		onSwipe: function(page,direction){

			switch(page){
				case 'calendar':
					switch(direction){
						case 'left':
							
							$.mobile.changePage("calendarAbsencesToApprove.html", { transition: "slide", reloadPage: true} );
							
							break;
						case 'right':
							//nothing
							break;
					}
					break;
				
				case 'calendarAbsencesToApprove':
					switch(direction){
						case 'left':
							$.mobile.changePage( "calendarStatistics.html", { transition: "slide", reloadPage: true } );
							break;
						case 'right':
							$.mobile.changePage( "calendar.html", { transition: "slide", reverse: true , reloadPage: true} );
							break;					
					}
					break;
				case 'calendarAbsencesStatistics':
					switch(direction){
						case 'left':
							//nothing
							break;
						case 'right':
							$.mobile.changePage("calendarAbsencesToApprove.html", { transition: "slide", reverse: true  , reloadPage: true} );
							break;				
					}
					break
			}
		},

		onChangeConnectivity: function(){
			changeInternetStatus()
		},
		
		getBalance: function(){
			if(internetStatus != 'none'){
				$.ajax({ 
					type: "GET",
					url:'',  
					dataType: "xml", 
					success:function(xml) {
						var balance = $(xml).find('Balance').text()
						var timestamp = $(xml).find('Date').text()
						var date = new Date(timestamp)
						//returns months from 0 to 11
						var month = date.getUTCMonth()+1
						var updateDate = date.getUTCDate() + "/"
														   + pad(month,2)
														   + "/" 
														   + date.getUTCFullYear() 
														   + " at " 
														   + pad(date.getUTCHours(),2)
														   + ":" 
														   + pad(date.getUTCMinutes(),2)
														   + ":" 
														   + pad(date.getUTCSeconds(),2)
						
						window.localStorage.setItem("balance", balance)
						window.localStorage.setItem("balanceLastUpdate", updateDate)
						

						
						$("#getBalance").text(balance + " €")
						$("#balanceUpdated").text("Last update: " + updateDate)
					}
				});
			}
			else{
				var balance = window.localStorage.getItem("balance")
				var updateDate = window.localStorage.getItem("balanceLastUpdate")					
				
				$("#getBalance").text(balance + " €")
				$("#balanceUpdated").text("Last update: " + updateDate)
			}
		}
};

//'max' is the number of digits to be shown
function pad(str, max) {
	str = str.toString()
	return str.length < max ? pad("0" + str, max) : str
}

function changeInternetStatus(){
	var parentElement = document.getElementById('deviceready')
	var listeningElement = parentElement.querySelector('.listening')
	var receivedLigadoElement = parentElement.querySelector('.receivedLigado')
	var receivedDesligadoElement = parentElement.querySelector('.receivedDesligado')

	internetStatus = navigator.connection.type

	if(internetStatus == 'none'){
		listeningElement.setAttribute('style', 'display:none;')
		receivedDesligadoElement.setAttribute('style', 'display:block;')
		receivedLigadoElement.setAttribute('style', 'display:none;')
	}
	else{
		listeningElement.setAttribute('style', 'display:none;')
		receivedLigadoElement.setAttribute('style', 'display:block;')
		receivedDesligadoElement.setAttribute('style', 'display:none;')
	}
}

function getNews() {	
	$('#loading').show()
	var client = new WindowsAzure.MobileServiceClient('https://ivanfreire.azure-mobile.net/', 'jTVwcFoetuaiwJYdSlyqRQJTxHdIJp31')

	var newsTable = client.getTable('news')
	newsTable.read().then(function(newsItems) {
		var listItems = $.map(newsItems, function(item) {
			var newNewsBox = $('<div class="newsBox">')
			.append($('<img class="imageBox">').attr('src', item.imageurl))
			.append($('<h5 class="newsTitle">').html(item.title))
			.append($('<p class="newsText">').text(item.content));

			return $('<li style="margin-left:-35px;">').append(newNewsBox)
		});

		var padding = $('<li style="margin-left:-35px;">').append($('<div style="height:35px">'));
		$('#loading').hide()
		$('#newsList').empty().append(listItems)
		$('#newsList').append(padding)
	});
}

function blockNewLinesInTextBoxes(){
	$("#noteNewAbsence").keydown(function(e){
		if (e.keyCode == 13 && !e.shiftKey)
		{
			// prevent default behavior
			e.preventDefault();
			return false;
		}
	});
}

function transaction_error(tx, error) {
    alert("Database Error: " + error);
}

function populateDB_success() {
	dbCreated = true;
    db.transaction(getAbsences, transaction_error);
}

function getAbsences(tx) {
	if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendar.html'){
		var sql = "SELECT * FROM absences GROUP BY id ORDER BY absenceFrom";
		tx.executeSql(sql, [], getAbsences_success);
	}
	if(location.pathname.substring(location.pathname.lastIndexOf("/") + 1) === 'calendarAbsencesToApprove.html'){
		var sql = "SELECT * FROM absences WHERE approved='pending' GROUP BY id ORDER BY absenceFrom";
		tx.executeSql(sql, [], getAbsencesToApprove_success);
	}
	
	
}

function getAbsences_success(tx, results) {
    var len = results.rows.length;
    numAbsences = len;
    if(internetStatus != 'none')
    	$('#absencesList').empty()
    
    for (var i=0; i<len; i++) {
    	var absence = results.rows.item(i);
		var absenceType = absence.type
		var absenceText = absence.note
		var absenceDateFrom = absence.absenceFrom
		var absenceDateTo = absence.absenceTo
		
		var statusColor;
		if(absence.approved === 'true'){
			statusColor='green';
			statusText='Approved'
		}
		else if(absence.approved === 'false'){
			statusColor='red';
			statusText='Rejected'
		}
		else{
			statusColor='orange';
			statusText='Pending approvement'
		}
		
		var typeColor = getTypeColor(absenceType)
		
		
		var yyyyFrom = absenceDateFrom.split("-")[0];           
		var mmFrom = absenceDateFrom.split("-")[1];
		var ddFrom = absenceDateFrom.split("-")[2];
		
		var yyyyTo = absenceDateTo.split("-")[0];           
		var mmTo = absenceDateTo.split("-")[1];
		var ddTo = absenceDateTo.split("-")[2];
		
		var newAbsenceBox = $('<div class="absenceBox" >')
				.append($('<div class="absenceDateBox" id="absenceDateBox' + absence.id + '" style="border: 1px solid grey; background-color:' + typeColor + ';">')
				.append($('<span style="color:white;">').text(ddFrom + "/" + mmFrom)))
				.append($('<div class="absenceText">').text(absenceText))
		
				
		var newAbsenceBoxItem = $('<li id="absenceInfoItem' + absence.id + '" style="margin-left:-35px;">').append(newAbsenceBox)
		
		$('#absencesList').append(newAbsenceBoxItem)
		
		
		
		var newAbsenceInfoBoxItem = $('<li id="hiddenInfo' + absence.id + '" style="margin-left:-35px; display:none; ">')
			.append($('<div style="text-align:left; float:left; position:relative; margin-bottom:10px; border-bottom: 2px solid black; border-left: 2px solid black; padding-left:5px;">')
					.append($('<span">').text("Status: ")).append($('<span style="color:' + statusColor + ';">').text(statusText).append('<br>'))
					.append($('<span>').text("Type: ")).append($('<span style="color:' + typeColor + ';">').text(absenceType).append('<br>'))
					.append($('<span>').text("Absence from ")).append($('<span style="font-weight: bold;">').text(ddFrom + '/' + mmFrom + '/' + yyyyFrom))
					.append($('<span>').text(" to ")).append($('<span style="font-weight: bold;">').text(ddTo + '/' + mmTo + '/' + yyyyTo)))					

	    $('#absencesList').append(newAbsenceInfoBoxItem)
	    
				
				

		$('#absenceInfoItem' + absence.id + '').click(function() {			
			var selectedID = this.id.substring(15); 
			var container = $('#absencesListContainer')
			var scrollTo = $('#absenceInfoItem' + selectedID + '')
			hideAllOtherInfo(selectedID)
			
				
			
			$('#hiddenInfo' + selectedID + '').slideToggle()	
			container.animate({
				scrollTop: scrollTo.offset().top - container.offset().top + container.scrollTop()
			});
		});	
			



		
		$('#absencesList').show()

    }
	
}

function getAbsencesToApprove_success(tx, results) {
    var len = results.rows.length;
    if(internetStatus != 'none')
    	$('#absencesToApproveList').empty()	
    for (var i=0; i<len; i++) {
    	var absence = results.rows.item(i);
		var absenceType = absence.type
		var absenceText = absence.note
		var absenceDateFrom = absence.absenceFrom
		var absenceDateTo = absence.absenceTo
        var absencePhoto = absence.picture
	
        var statusColor;
		if(absence.approved === 'true')
			statusColor='green';
		else if(absence.approved === 'false')
			statusColor='red';
		else
			statusColor='orange';
		
		var typeColor = getTypeColor(absenceType)
		
		
		var yyyyFrom = absenceDateFrom.split("-")[0];           
		var mmFrom = absenceDateFrom.split("-")[1];
		var ddFrom = absenceDateFrom.split("-")[2];
		
		var yyyyTo = absenceDateTo.split("-")[0];           
		var mmTo = absenceDateTo.split("-")[1];
		var ddTo = absenceDateTo.split("-")[2];
	
		var newAbsenceBox = $('<div class="absenceBox" >')
						.append($('<div class="absenceDateBox" id="absenceDateBox' + absence.id + '" style="border: 1px solid grey; background-color:' + typeColor + ';">')
						.append($('<span style="color:white;">').text(ddFrom + "/" + mmFrom)))
						.append($('<div id="absenceText' + absence.id + '" class="absenceText">').text(absenceText))
                        .append($('<p id="absenceDateFrom' + absence.id + '" class="absenceText" style="display:none;">').text(absenceDateFrom))
                        .append($('<p id="absenceDateTo' + absence.id + '" class="absenceText" style="display:none;">').text(absenceDateTo))
                        .append($('<p id="absenceType' + absence.id + '" class="absenceText" style="display:none;">').text(absenceType))
                        .append($('<img id="absencePhoto' + absence.id + '" class="absenceText" style="display:none;">').attr("src",absencePhoto)); 
		
		var newAbsenceBoxItem = $('<li id="absenceItem' + absence.id + '" style="margin-left:-35px;">').append(newAbsenceBox)		
		$('#absencesToApproveList').append(newAbsenceBoxItem)

		
		$('#absenceItem' + absence.id + '').click(function(){
            var selectedID = this.id.substring(11); 
            var photoUrl = $("#absencePhoto" + selectedID ).attr('src') 

            $('.absencePhoto').attr("src",photoUrl)      
			$('.noteAbsence').text($("#absenceText" + selectedID ).text())
		    $('#addAbsenceBoxOuter2').show()
			$('.approveAbsenceBox').show()
			 
			$('#closeToApproveAbsence').click(function () {
				$('#addAbsenceBoxOuter2').hide()
			    $('.approveAbsenceBox').hide()
			});
			
	        $('#approveAbsence').click(function () {
                $('#approveAbsence').off('click')
			    $('#addAbsenceBoxOuter2').hide()
			    $('.approveAbsenceBox').hide()
				$('.absencePhoto').empty()
				$('.noteAbsence').empty()
				$('#rejectAbsence').off('click')
				db.transaction( 
						function(tx){
							tx.executeSql("DELETE FROM absences WHERE id=" + selectedID +";");
						});
							
				db.transaction(
						function(tx) {        
							tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" 
									+ selectedID  				    		                                                                                          
									+ ",'" 				    		                                                                                        
									+ $("#absenceType" + selectedID ).text()				    		
									+ "','"				    		
									+ $("#absenceDateFrom" + selectedID ).text()					    		
									+ "','" 					    		
									+ $("#absenceDateTo" + selectedID ).text()				    		
									+ "','" 					    		
									+ $("#absenceText" + selectedID ).text()					    		
									+ "'," 					    		
									+ "'true'"  
									+ ",'" 					    		
									+ $("#absencePhoto" + selectedID ).attr('src') 					    		
									+ "')");
						}, transaction_error);
				
				db.transaction(getAbsences, transaction_error);			
	        })    			
			$('#rejectAbsence').click(function () {		
			    $('#addAbsenceBoxOuter2').hide()
			    $('.approveAbsenceBox').hide()
				$('.absencePhoto').empty()
				$('.noteAbsence').empty()
                $('#approveAbsence').off('click')
                db.transaction( 
						function(tx){
							tx.executeSql("DELETE FROM absences WHERE id=" + selectedID +";");
						});
							
				db.transaction(
						function(tx) {        
							tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" 
									+ selectedID  				    		                                                                                          
									+ ",'" 				    		                                                                                        
									+ $("#absenceType" + selectedID ).text()				    		
									+ "','"				    		
									+ $("#absenceDateFrom" + selectedID ).text()					    		
									+ "','" 					    		
									+ $("#absenceDateTo" + selectedID ).text()				    		
									+ "','" 					    		
									+ $("#absenceText" + selectedID ).text()					    		
									+ "'," 					    		
									+ "'false'"  
									+ ",'" 					    		
									+ $("#absencePhoto" + selectedID ).attr('src') 					    		
									+ "')");
						}, transaction_error);
				
				db.transaction(getAbsences, transaction_error);	
	        })
	        		
        })
    }
    $('#absencesToApproveList').show()
	
}

function populateDB(tx) {
	tx.executeSql('DROP TABLE IF EXISTS absences');
	var sql = 
		"CREATE TABLE IF NOT EXISTS absences ( "+
		"id INTEGER PRIMARY KEY, " +
		"type VARCHAR(50), " +
		"absenceFrom VARCHAR(50), " +
		"absenceTo VARCHAR(50), " +
		"note VARCHAR(50), " + 
		"approved VARCHAR(50), " +
		"picture VARCHAR(200))";
    tx.executeSql(sql);
    

    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Doença','2014-01-05','2014-01-09','Estou doente.','true','images/ray_moore.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Gravidez','2014-04-07','2014-06-07','Licença de parto','pending','images/lisa_wong.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Doença','2014-02-09','2014-02-09','Não me estou a sentir bem.','false','images/john_williams.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Assistência a familia','2014-03-03','2014-03-08','Tenho de ficar em casa uns dias por causa do meu filho.','true','images/lisa_wong.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Prova de avaliação','2014-07-28','2014-07-05','Tenho um exame.','true','images/john_williams.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Férias','2014-08-01','2014-08-19','Preciso de umas ferias.','pending','images/ray_moore.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Falecimento','2014-02-03','2014-02-07','Falecimento de um familiar.','true','images/john_williams.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Outro','2014-02-03','2014-02-03','Tenho de comparecer em tribunal.','true','images/lisa_wong.jpg')");
    tx.executeSql("INSERT INTO absences (id,type,absenceFrom,absenceTo,note,approved,picture) VALUES (" + id++ + ",'Férias','2014-08-20','2014-08-25','Mais uns dias de férias','false','images/ray_moore.jpg')");
}

function getTypeColor(type){
	//#FFA500 orange
	//#9ACD32 green
	//#20B2AA blue
	//#FFA07A light salmon
	//#CD853F brown
	//#DDA0DD purple
	
	switch(type){
	case 'Férias':
		return '#FFA500';
	case 'Doença':
		return '#FF6347';
	case 'Assistência a familia':
		return '#20B2AA'
	case 'Falecimento':
		return '#000000'
	case 'Gravidez':
		return '#FFA07A'
	case 'Prova de avaliação':
		return '#DDA0DD'
	case 'Outro':
		return '#CD853F'
	}
}


function drawChart(sql){
//	alert(sql)
	db.transaction(function (tx){
		tx.executeSql(sql, [], function (tx, results) {
			var len = results.rows.length;
			if(len==0){
				var absencesPercentage =  new Array(1)
				var chartColors = new Array(1)
				absencesPercentage[0] = ['No Value', 100]         	
				chartColors[0] = 'grey';
				$('#statisticsChart').highcharts({
					chart: {
						plotBackgroundColor: null,
						plotBorderWidth: null,
						plotShadow: false
					},
					tooltip: {
						pointFormat: '<b>{point.percentage:.1f}%</b>'
					},
					title: {
						text: 'Empty chart. Choose different filters.'
					},
					plotOptions: {
						pie: {
							allowPointSelect: true,
							cursor: 'pointer',
							dataLabels: {
								enabled: false,
							},
							showInLegend: false,
							colors: chartColors,
							allowPointSelect: true,
							point:{
								events : {
									legendItemClick: function(e){
										e.preventDefault();
									}
								}
							}
						}
					},
					series: [{
						type: 'pie',
						name: 'No value',
						data: absencesPercentage
					}]
				});
					
					
			}
			else{
				var absencesPercentage =  new Array(len-1)
				var chartColors = new Array(len-1)
				for (var i=0; i<len; i++) {
					var absence = results.rows.item(i);
					var typeColor = getTypeColor(absence.type)

					absencesPercentage[i] = [absence.type, absence.typeCount]         	
					chartColors[i] = typeColor
				}

				$('#statisticsChart').highcharts({
					chart: {
						plotBackgroundColor: null,
						plotBorderWidth: null,
						plotShadow: false
					},
					tooltip: {
						pointFormat: '<b>{point.percentage:.1f}%</b>'
					},
					title: {
						text: '',
						style: {
							display: 'none'
						}
					},
					plotOptions: {
						pie: {
							allowPointSelect: true,
							cursor: 'pointer',
							dataLabels: {
								enabled: false,
							},
							showInLegend: false,
							colors: chartColors,
							allowPointSelect: true,
							point:{
								events : {
									legendItemClick: function(e){
										e.preventDefault();
									}
								}
							}
						}
					},
					series: [{
						type: 'pie',
						name: 'Absence %',
						data: absencesPercentage
					}]
				});


			}
		});
	});

}

function hideAllOtherInfo(id){
	for(i=1; i<=numAbsences; i++){
		if($('#hiddenInfo' + i + '').is(":visible"))
			if(i!=id)
				$('#hiddenInfo' + i + '').slideToggle()

	}
}










