<template>
	<div id="rotate">
		<span
			v-for="(title, index) in titles.GOODs"
			:key="index"
			:title="title.value"
			:style="{ color: title.color || '#ffffff' }"
			@click="showEdit(title)"
		>
			{{ title.value }}
		</span>
	</div>
</template>

<script>
// import interfaceUrl from "@/api/interfaceUrl";
// import { publicInter } from "@/api/http";
export default {
	name: "rotate",
	props: ["intellNum"],
	data() {
		return {
			radius: 90,
			dtr: Math.PI / 180,
			d: 220,
			tspeed: 0.4,
			mouseX: 0,
			mouseY: 0,
			lasta: 1,
			lastb: 1,
			mcList: [],
			aSpan: [],
			oDiv: null,
			timer: null,
			titles: { GOODs: [], key: "" },
			arr: [
				{
					UUID: "96325D43EBF2E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION: "硫化氢气体泄漏监测",
					GOODSDESCRIPTIONCN: "928硫化氢气体泄漏探测",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120036_展品_54.jpg",
					POSITION: "4.1A2-001",
				},
				{
					UUID: "96325D43EBF3E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"该生产线由宝力独立集成，最新推出一款由料架与机械手融合而成的智能料库， 使安装效率获得跨越式突破，作为该单元的中轴连接环节，把生产自动化升级控制在数小时之内。",
					GOODSDESCRIPTIONCN: "日本【Roku-Roku+Brother+Fanuc】柔性加工单元",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201904045495_展品_662.png",
					POSITION: "3A2-001",
				},
				{
					UUID: "96325D43EBF4E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"BLM集团 - 装配主动扫描装置ACTIVE SCAN的LT FIBER激光切管机BLM集团是全球首家设计和制造金属管材激光切割设备的公司（1986年），拥有超过30年的行业经验。",
					GOODSDESCRIPTIONCN: "LT FIBER激光切管机",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201905154940_展品_818.jpg",
					POSITION: "3C2-001",
				},
				{
					UUID: "96325D43EBF5E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"有机硅材料的3D打印是行业目前最新的研究方向， 本次展会计划通过现场安装打印机设备以支持有机硅材料3D打印的现场演示。",
					GOODSDESCRIPTIONCN: "有机硅3D打印机",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201905244762_展品_860.jpg",
					POSITION: "3C7-003",
				},
				{
					UUID: "96325D43EBF6E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"大棕熊100飞机（Kodiak 100）是美国QUEST飞机公司生产制造的一款设计先进、安全可靠、性能优异的10座单发涡桨多用途飞机。",
					GOODSDESCRIPTIONCN: "大棕熊100飞机（Kodiak 100）",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201906108995_展品_896.jpg",
					POSITION: "4.1C3-002",
				},
				{
					UUID: "96325D43EBF7E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"Bumotec s191是可信赖的“瑞士制造”和最新的直线电机驱动技术完美结合的产物，浇铸铸铁床身和导轨以及高静态质量所带来的机床刚性，为无振动加工提供了坚实的保证。",
					GOODSDESCRIPTIONCN: "5轴联动车铣复合加工中心",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201907024875_展品_919.png",
					POSITION: "3C4-002",
				},
				{
					UUID: "96325D43EBF8E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"阿尔法·罗密欧2018年F1大奖赛参赛车型1:1等比例复刻赛车模型",
					GOODSDESCRIPTIONCN: "阿尔法·罗密欧F1 C37赛车模型",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201807134865_展品_99.JPG",
					POSITION: "3C8-010",
				},
				{
					UUID: "96325D43EC05E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"法拉帝FSD 195高速巡逻船全长20米，重约37吨，配备两台1900马力MAN发动机，最高速度超过55节，以35节的巡航速度能够续航525海里以上。",
					GOODSDESCRIPTIONCN: "法拉帝FSD 195 高速巡逻船",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805170116_展品_202.jpg",
					POSITION: "4.1C4-001（003）",
				},
				{
					UUID: "96325D43EC0EE871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"新式立柱的结构设计以及极轻质的材料，使立柱最大高度可达到25m并具有极高的稳定性，内置集成的Omega驱动装置和创新的导轨概念不仅最大化缩小了安全距离，还大大增加了储存空间。",
					GOODSDESCRIPTIONCN: "Miniload料箱堆垛机",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120039_展品_72.jpg",
					POSITION: "4.1C4-002",
				},
				{
					UUID: "96325D43EC0FE871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"AT37-G绝缘斗臂车为美国原装整车进口，为满足中国电力用户市场需求，底盘选用可乘坐6人的双排座福特猛禽F550底盘，市场占有率绝对优势。",
					GOODSDESCRIPTIONCN: "无支腿型绝缘斗臂车",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201905054072_展品_781.png",
					POSITION: "0A1-001",
				},
				{
					UUID: "96325D43EC10E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"阿尔泰克AT40-GW履带式自行走绝缘斗臂车，整车美国原装进口,绝缘等级符合ANSI标准46kV及以下，最大工作高度13.3米，混合臂折叠+伸缩，保证绝缘性能的同时满足OSHA安全标准。",
					GOODSDESCRIPTIONCN: "履带式绝缘斗臂车",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201905054072_展品_782.jpg",
					POSITION: "0A1-001",
				},
				{
					UUID: "96325D43EC11E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"高品质、高稳定性，高生产率，具有卓越且亲和力的操控性，，功率可达11/7.5KW（10分钟/连续）。",
					GOODSDESCRIPTIONCN: "卧式数控车床 GENOS L250Ⅱ-e",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120015_展品_4.jpg",
					POSITION: "3B1-001",
				},
				{
					UUID: "96325D43EC12E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"通过与飞机制造商和发动机制造商的紧密合作，利勃海尔生产飞控制统和空气管理系统，为多款飞机提供起落架并参与发动机项目。",
					GOODSDESCRIPTIONCN: "C919飞机前起落架",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120800_展品_181.jpg",
					POSITION: "3B3-001",
				},
				{
					UUID: "96325D43EC13E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"作为AW109直升机系列的最新成员，AW109 Trekker以同级别最出色的高温高海拔性能，超远航程和超长续航时间和出色的经济性，成为了完成培训、通航作业和其他飞行任务的利器。",
					GOODSDESCRIPTIONCN: "AW109 Trekker探路者直升机",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/Z201901215981_展品_562.jpg",
					POSITION: "0A1-007",
				},
				{
					UUID: "96325D43EC14E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"烟烙尽(IG541)气体灭火系统使用来源大气的天然灭火剂：52%氮气、40%氩气和8%二氧化碳，完全无毒；不损害敏感电子设备和不可替代物品。\n\n",
					GOODSDESCRIPTIONCN: "安素INERGEN烟烙尽洁净气体灭火产品",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120053_展品_101.jpg",
					POSITION: "4.1B1-004",
				},
				{
					UUID: "96325D43EC15E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"YVAG全变频风冷冷水/热泵机组是一款超高效家用水系统中央空调室外机，有着超低运转噪音以及稳定可靠的品质。",
					GOODSDESCRIPTIONCN: "约克YVAG全变频风冷冷水/热泵机组",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/C201805120053_展品_105.jpg",
					POSITION: "4.1B1-004",
				},
				{
					UUID: "96325D43EC26E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"意大利莱奥纳多直升机的中国首秀，价值2亿元人民币。目前全球最新型的高性能8.6吨超级中型双发直升机，满载最大巡航速度为287公里/小时。",
					GOODSDESCRIPTIONCN: "A109S直升机(SN-22709)",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/helicopter.jpg",
					POSITION: "4.1C2-001",
				},
				{
					UUID: "96325D43EC27E871E053650C10AC9048",
					EXHTYPE: "3",
					GOODSDESCRIPTION:
						"第五代FORPHEUS具有增强人工智能教练功能以及出色的摄像系统，可以预测玩家的行为及回球动作，而新型机器人的“肘部和腕部”运动可以打出上旋和下旋球，能够进行更专业的乒乓球训练。",
					GOODSDESCRIPTIONCN: "乒乓球机器人“FORPHEUS”",
					TOTALPRICE: -1,
					REC_UPD_DT: "2019-10-31",
					IMAGEPATH: "/featureExhi/robot.jpg",
					POSITION: "4.1B5-001",
				},
			],
		};
	},
	mounted() {
		this.radius = document.getElementById("rotate").offsetWidth / 3;
		clearInterval(this.timer);
		this.tabIntell();
		this.init();
	},
	watch: {
		"titles.key": function (val) {
			clearInterval(this.timer);
			setTimeout(() => {
				this.init();
			}, 30);
		},
	},
	methods: {
		showEdit(params) {
			// if(params.color == "#FFE91A"){
			//     this.$emit('showEdit',params);
			// }
			// console.log(params);
		},
		tabIntell(value, index) {
			this.title = { key: value, value: this.titles[value] };
			let params = {
				exhType: value,
				// highNum:10,
				highNum: 6,
				lowNum: 10,
			};
			var colors = [
				"#43C5FF",
				"#8FA1FF",
				"#FFFFFF",
				"#FF9A55",
				"#FF7676",
				"#FF9A55",
				"#FFFFFF",
				"#5FB3FF",
				"#8493EC",
			];
			this.titles.GOODs = [];
			for (let i = 0; i < this.arr.length; i++) {
				let unit = {};
				if (this.arr[i].TOTALPRICE == -1) {
					unit.color = "#FFE91A";
					unit.value = this.arr[i].GOODSDESCRIPTIONCN;
					unit.EXHTYPE = this.arr[i].EXHTYPE;
					unit.UUID = this.arr[i].UUID;
					this.titles.GOODs.push(unit);
				}
			}
			this.titles.key = value;
			// publicInter(interfaceUrl.queryGoodsByExhType, params).then((r) => {
			// 	if (r) {
			// 		if (r.isOk || r.isOk == "true") {
			// 			// "#FFE91A"黄色
			// 			var colors = [
			// 				"#43C5FF",
			// 				"#8FA1FF",
			// 				"#FFFFFF",
			// 				"#FF9A55",
			// 				"#FF7676",
			// 				"#FF9A55",
			// 				"#FFFFFF",
			// 				"#5FB3FF",
			// 				"#8493EC",
			// 			];
			// 			this.titles.GOODs = [];

			// 			// for (let i = 0; i < r.highGoods.length; i++) {
			// 			// 	let unit = {};
			// 			// 	unit.color = colors[i % 9];
			// 			// 	// unit.value = r.highGoods[i].GOODSDESCRIPTIONCN+' '+parseInt(r.highGoods[i].TOTALPRICE/10000)+"万美元";
			// 			// 	unit.value = r.highGoods[i].GOODSDESCRIPTIONCN;
			// 			// 	unit.EXHTYPE = r.highGoods[i].EXHTYPE;
			// 			// 	unit.UUID = r.highGoods[i].UUID;
			// 			// 	this.titles.GOODs.push(unit);
			// 			// }
			// 			for (let i = 0; i < r.lowGoods.length; i++) {
			// 				let unit = {};
			// 				if (r.lowGoods[i].TOTALPRICE == -1) {
			// 					unit.color = "#FFE91A";
			// 					unit.value = r.lowGoods[i].GOODSDESCRIPTIONCN;
			// 					unit.EXHTYPE = r.lowGoods[i].EXHTYPE;
			// 					unit.UUID = r.lowGoods[i].UUID;
			// 					this.titles.GOODs.push(unit);
			// 				}
			// 			}
			// 			this.titles.key = value;
			// 		} else {
			// 			this.$Message.error(r.msg);
			// 		}
			// 	}
			// });
		},
		init() {
			this.oDiv = document.getElementById("rotate");
			this.aSpan = this.oDiv.getElementsByTagName("span");
			this.mcList = [];
			for (let i = 0; i < this.aSpan.length; i++) {
				let oTag = {};
				oTag.offsetWidth = this.aSpan[i].offsetWidth;
				oTag.offsetHeight = this.aSpan[i].offsetHeight;
				this.mcList.push(oTag);
			}
			console.log("mclist", this.mcList);
			this.positionAll();
			this.timer = setInterval(() => {
				this.update();
			}, 41);
		},
		update() {
			let a, b;
			a = -this.lasta * 1.2 * this.tspeed;
			b = -this.lastb * 1.2 * this.tspeed;

			// this.lasta = a;
			// this.lastb = b;

			if (Math.abs(a) <= 0.01 && Math.abs(b) <= 0.01) {
				return;
			}

			let sa = Math.sin(a * this.dtr),
				ca = Math.cos(a * this.dtr),
				sb = Math.sin(b * this.dtr),
				cb = Math.cos(b * this.dtr),
				sc = Math.sin(0 * this.dtr),
				cc = Math.cos(0 * this.dtr);

			for (let j = 0; j < this.mcList.length; j++) {
				let rx1 = this.mcList[j].cx;
				let ry1 = this.mcList[j].cy * ca + this.mcList[j].cz * -sa;
				let rz1 = this.mcList[j].cy * sa + this.mcList[j].cz * ca;

				let rx2 = rx1 * cb + rz1 * sb;
				let ry2 = ry1;
				let rz2 = rx1 * -sb + rz1 * cb;

				let rx3 = rx2 * cc + ry2 * -sc;
				let ry3 = rx2 * sc + ry2 * cc;
				let rz3 = rz2;

				this.mcList[j].cx = rx3;
				this.mcList[j].cy = ry3;
				this.mcList[j].cz = rz3;

				let per = this.d / (this.d + rz3);

				this.mcList[j].x =
					this.howElliptical * rx3 * per - this.howElliptical * 2;
				this.mcList[j].y = ry3 * per;
				this.mcList[j].scale = per / 1.5;
				this.mcList[j].alpha = per;

				this.mcList[j].alpha = (this.mcList[j].alpha - 0.6) * (10 / 6);
			}

			this.doPosition();
			this.depthSort();
		},
		depthSort() {
			let aTmp = [];

			for (let i = 0; i < this.aSpan.length; i++) {
				aTmp.push(this.aSpan[i]);
			}

			aTmp.sort(function (vItem1, vItem2) {
				if (vItem1.cz > vItem2.cz) {
					return -1;
				} else if (vItem1.cz < vItem2.cz) {
					return 1;
				} else {
					return 0;
				}
			});

			for (let i = 0; i < aTmp.length; i++) {
				aTmp[i].style.zIndex = i;
			}
		},
		doPosition() {
			let l = this.oDiv.offsetWidth / 2;
			let t = this.oDiv.offsetHeight / 2;
			for (var i = 0; i < this.mcList.length; i++) {
				this.aSpan[i].style.left =
					this.mcList[i].cx + l - this.mcList[i].offsetWidth / 2 + "px";
				this.aSpan[i].style.top =
					this.mcList[i].cy + t - this.mcList[i].offsetHeight / 2 + "px";

				// this.aSpan[i].style.fontSize=Math.ceil(12*this.mcList[i].scale/2)+8+'px';

				this.aSpan[i].style.filter =
					"alpha(opacity=" + 100 * this.mcList[i].alpha + ")";
				this.aSpan[i].style.opacity = this.mcList[i].alpha;
			}
		},
		positionAll() {
			let phi = 0,
				theta = 0,
				max = this.mcList.length,
				i = 0,
				aTmp = [],
				oFragment = document.createDocumentFragment();

			for (let i = 0; i < this.aSpan.length; i++) {
				aTmp.push(this.aSpan[i]);
			}

			aTmp.sort(function () {
				return Math.random() < 0.5 ? 1 : -1;
			});

			for (let i = 0; i < aTmp.length; i++) {
				oFragment.appendChild(aTmp[i]);
			}

			this.oDiv.appendChild(oFragment);

			for (let i = 1; i < max + 1; i++) {
				phi = Math.acos(-1 + (2 * i - 1) / max);
				theta = Math.sqrt(max * Math.PI) * phi;

				this.mcList[i - 1].cx = this.radius * Math.cos(theta) * Math.sin(phi);
				this.mcList[i - 1].cy = this.radius * Math.sin(theta) * Math.sin(phi);
				this.mcList[i - 1].cz = this.radius * Math.cos(phi);
				this.aSpan[i - 1].style.left =
					this.mcList[i - 1].cx +
					this.oDiv.offsetWidth / 2 -
					this.mcList[i - 1].offsetWidth / 2 +
					"px";
				this.aSpan[i - 1].style.top =
					this.mcList[i - 1].cy +
					this.oDiv.offsetHeight / 2 -
					this.mcList[i - 1].offsetHeight / 2 +
					"px";
			}
		},
		// mouseoverFun(){
		//     clearInterval(this.timer);
		//     this.timer = null;
		// },
		// mouseoutFun(){
		//     this.timer = setInterval(()=>{this.update()},30);
		// }
	},
	beforeDestroy: function () {
		clearInterval(this.timer);
		this.timer = null;
	},
};
</script>



<style scoped>
#rotate {
	position: relative;
	margin: 0 auto;
	width: 927px;
	height: 238px;
}
#rotate span {
	position: absolute;
	top: 0;
	left: 0;
	font-family: "Microsoft YaHei";
	padding: 3px 6px;
	max-width: 180px;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
	cursor: pointer;
	font-size: 1rem;
}
</style>