# GCPhone

This is not my work. I simply translated to English and ensured it works properly on mysql 3.X
-----------------------------------------------------------------------------------------------
    Download the gcphone and esx_addons_gcphone
    Put the downloaded files on your ftp.
        server-data/resources/
    In, your server.cfg, put before the jobs addons :
        start gcphone
        start esx_addons_gcphone

    Configure the voice, in gcphone/html/static/config/config.json.
        Basic use "useWebRTCVocal": false, unless you have a custom RTC server.

	"//": "useWebRTCVocal: false => Appels avec channels de GTA",
	"//": "useWebRTCVocal: true  => Appels avec WebRTC",
	"useWebRTCVocal": false,
	"RTCConfig": {
		"iceServers": [{
		"urls": ["turn:gannon.ovh"],
		"username": "jojo",
		"credential": "pass"
		}]
	},

    
####Change the default zoom (not tested)####

In gcphone/html/static/config/config.json, add this : "zoom" : "60%",

Or in your html/static/js/app.js search zoom: window.localStorage.gc_zoom || "100%", and replace by zoom: window.localStorage.gc_zoom || "60%",

Now, clear your server cache and maybe your FiveM cache.

For use distress signal (esx_ambulancejob), you need to edit the client.main : Replace :

function SendDistressSignal()
	local playerPed = PlayerPedId()
	local coords	= GetEntityCoords(playerPed)

	ESX.ShowNotification(_U('distress_sent'))
	TriggerServerEvent('esx_phone:send', 'ambulance', _U('distress_message'), false, {
		x = coords.x,
		y = coords.y,
		z = coords.z
	})
end

By this :

function SendDistressSignal()
	local playerPed = PlayerPedId()
	PedPosition		= GetEntityCoords(playerPed)
	
	local PlayerCoords = { x = PedPosition.x, y = PedPosition.y, z = PedPosition.z }

	ESX.ShowNotification(_U('distress_sent'))

    TriggerServerEvent('esx_addons_gcphone:startCall', 'ambulance', _U('distress_message'), PlayerCoords, {

		PlayerCoords = { x = PedPosition.x, y = PedPosition.y, z = PedPosition.z },
	})
end

For add custom message on phone :

			"display": "Police",
			"icon": "/html/static/img/icons_app/bank.png",
			"subMenu": [
				{
					"title": "Envoyer un message",
					"eventName": "esx_addons_gcphone:call",
					"type": {
						"number": "police"
					}
				},
				{
					"title": "Appeler le standard",
					"eventName": "gcphone:autoCallNumber",
					"type": {
						"number": "911"
					}
				},
				{
					"title": "Signaler un vol",
					"eventName": "esx_addons_gcphone:call",
					"type": {
						"number": "police",
						"message": "Vol en cours, merci de venir au plus vite !"
					}
				},
				{
					"title": "Signaler une agression",
					"eventName": "esx_addons_gcphone:call",
					"type": {
						"number": "police",
						"message": "Victime d'agression"
					}
				}
			]
