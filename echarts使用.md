
### 1.柱状图
h5

    <div id="_bar" style="height: 500px;  -webkit-tap-highlight-color: transparent; -webkit-user-select: none; cursor: default; background-color: rgba(0, 0, 0, 0);padding-top: 15px;">
js

	//初始化
	var bar_echarts = echarts.init(document.getElementById("_bar"));
	//option
	bar_option = {
			title: {
				text: "设备数",
			},
			tooltip: {
				trigger: "axis"
			},
			legend: {
				data: ["库存","销售"]
			},
			toolbox: {
				show: true,
				x: "right",
				feature: {
					dataView: {
						show: true,
						readOnly: false
					},
					magicType: {
						show: true,
						type: ["line", "bar", ]
					},
					"restore": {
						"show": true
					},
					"saveAsImage": {
						"show": true
					}
				}
			},
			calculable: true,
			xAxis: [{
				type: "category",
				data: []
			}],
			yAxis: [{
				type: "value"
			}],
			series: []
		};
	//异步获取数据
	function requestStore(ec,option){
			var requestUrl = "${baseURL}/index/showStore";

			ec.showLoading({
				text:'数据加载中...'
			});

			$.ajax({
				url:requestUrl,  
		        type:'get',  
		        dataType:'json',  
		        success:function(jsons){  
		        	ec.hideLoading();
		            var Item = function(){  
		                return {  
		                    name:'',  
		                    type:'bar',  
		                    data:[]  
		                    }  
		                };  
		            var legends = [];  
		            var Series = [];  
		            var json = jsons;  
		           console.log("store:"+jsons);
	                var it = new Item();  
	                it.data = json.Ydata1;  
	                it.name = "库存"; 
	                Series.push(it); 

	                  var it2 = new Item();  
	                it2.data = json.Ydata2;  
	                it2.name = "销售"; 
	                Series.push(it2); 
		                
		            option.xAxis[0].data = jsons.Xdata;   
		             
		            option.series = Series; // 设置图表  
		            ec.setOption(option);// 重新加载图表  
		            window.onresize = ec.resize;
		        },  
		        error:function(){  
		        	ec.hideLoading();
		        	console.log("requestStore error");
		           // alert("数据加载失败！请检查数据链接是否正确");  
		        }  
			});
		}
返回数据：

	{Ydata1: [19998, 0, 0, 0, 0], Ydata2: [2, 0, 0, 0, 0], Xdata: ["A1", "A10", "A18", "A19", "A5"]}

###2.饼状图
h5

	<div id="_pie" style="height: 500px; -webkit-tap-highlight-color: transparent; -webkit-user-select: none; cursor: default; background-color: rgba(0, 0, 0, 0);padding-top: 20px; ">
js

	pie_option = {
			title: {
				text: "库存占总数比",				
				x: "center"
			},
			tooltip: {
				trigger: "item",
				formatter: "{a} <br/>{b} : {c} ({d}%)"
			},
			legend: {
				orient: 'vertical',
				x: 'left',
				data: []
			},
			toolbox: {
				show: true,
				feature: {
					dataView: {
						show: true,
						readOnly: false
					},
					magicType: {
						show: true,
						type: ["pie", "funnel"],
						option: {
							funnel: {
								x: "25%",
								width: "50%",
								funnelAlign: "left",
								max: 1548
							}
						}
					},
					restore: {
						show: true
					},
					saveAsImage: {
						show: true
					}
				}
			},
			calculable: true,
			series: [{
				
				type: "pie",
				radius: "55%",
				center: ["50%", "60%"],
				data: [],
				itemStyle: {
					normal: {
						label: {
							show: true,
							formatter: "{b}:({d}%)"
						},
						labelLine: {
							show: true
						}
					}
				}
			}]
		};

	function requestPie(ec,option){
			var requestUrl = "${baseURL}/index/showPie";
			ec.showLoading({
				text:'数据加载中...'
			});
			$.ajax({
				url:requestUrl,  
		        type:'get',  
		        dataType:'json',  
		        success:function(jsons){  
		            ec.hideLoading();
		            var legends = [];  
		            var Series = [];  
		            var json = jsons;  
	                 console.log(json.legends)
		             option.legend.data = json.legends;
		           // option.xAxis[0].data = jsons.Xdata;   
		             console.log(json.series);
		            option.series = {
							name: "库存占比",
							type: "pie",
							radius: "55%",
							center: ["50%", "60%"],
							data: json.series,
							itemStyle: {
								normal: {
									label: {
										show: true,
										formatter: "{b}:({d}%)"
									},
									labelLine: {
										show: true
									}
								}
							}
						} // 设置图表  
		            ec.setOption(option);// 重新加载图表  
		            window.onresize = ec.resize;
		        },  
		        error:function(){  
		        	ec.hideLoading();
		        	console.log("requestPie error");
		          //  alert("数据加载失败！请检查数据链接是否正确");  
		        }  
			});
		}

返回数据
	
	{"legends":["A1","A10","A18","A19","A5"],"series":[{"name":"A1","value":19998},{"name":"A10","value":0},{"name":"A18","value":0},{"name":"A19","value":0},{"name":"A5","value":0}]}

###地图
h5

	{data: [{country: "中国", dayActive: 0, occupancy: "100.0%", count: 1, sum: 0}]}

js

			var nameMap = {
				'United States':'美国',
			    'Afghanistan':'阿富汗',
				'Singapore':'新加坡',
			    'Angola':'安哥拉',
			    'Albania':'阿尔巴尼亚',
			    'United Arab Emirates':'阿联酋',
			    'Argentina':'阿根廷',
			    'Armenia':'亚美尼亚',
			    'French Southern and Antarctic Lands':'法属南半球和南极领地',
			    'Australia':'澳大利亚',
			    'Austria':'奥地利',
			    'Azerbaijan':'阿塞拜疆',
			    'Burundi':'布隆迪',
			    'Belgium':'比利时',
			    'Benin':'贝宁',
			    'Burkina Faso':'布基纳法索',
			    'Bangladesh':'孟加拉国',
			    'Bulgaria':'保加利亚',
			    'The Bahamas':'巴哈马',
			    'Bosnia and Herzegovina':'波斯尼亚和黑塞哥维那',
			    'Belarus':'白俄罗斯',
			    'Belize':'伯利兹',
			    'Bermuda':'百慕大',
			    'Bolivia':'玻利维亚',
			    'Brazil':'巴西',
			    'Brunei':'文莱',
			    'Bhutan':'不丹',
			    'Botswana':'博茨瓦纳',
			    'Central African Republic':'中非共和国',
			    'Canada':'加拿大',
			    'Switzerland':'瑞士',
			    'Chile':'智利',
			    'China':'中国',
			    'Ivory Coast':'象牙海岸',
			    'Cameroon':'喀麦隆',
			    'Democratic Republic of the Congo':'刚果民主共和国',
			    'Republic of the Congo':'刚果共和国',
			    'Colombia':'哥伦比亚',
			    'Costa Rica':'哥斯达黎加',
			    'Cuba':'古巴',
			    'Northern Cyprus':'北塞浦路斯',
			    'Cyprus':'塞浦路斯',
			    'Czech Republic':'捷克共和国',
			    'Germany':'德国',
			    'Djibouti':'吉布提',
			    'Denmark':'丹麦',
			    'Dominican Republic':'多明尼加共和国',
			    'Algeria':'阿尔及利亚',
			    'Ecuador':'厄瓜多尔',
			    'Egypt':'埃及',
			    'Eritrea':'厄立特里亚',
			    'Spain':'西班牙',
			    'Estonia':'爱沙尼亚',
			    'Ethiopia':'埃塞俄比亚',
			    'Finland':'芬兰',
			    'Fiji':'斐',
			    'Falkland Islands':'福克兰群岛',
			    'France':'法国',
			    'Gabon':'加蓬',
			    'United Kingdom':'英国',
			    'Georgia':'格鲁吉亚',
			    'Ghana':'加纳',
			    'Guinea':'几内亚',
			    'Gambia':'冈比亚',
			    'Guinea Bissau':'几内亚比绍',
			    'Equatorial Guinea':'赤道几内亚',
			    'Greece':'希腊',
			    'Greenland':'格陵兰',
			    'Guatemala':'危地马拉',
			    'French Guiana':'法属圭亚那',
			    'Guyana':'圭亚那',
			    'Honduras':'洪都拉斯',
			    'Croatia':'克罗地亚',
			    'Haiti':'海地',
			    'Hungary':'匈牙利',
			    'Indonesia':'印尼',
			    'India':'印度',
			    'Ireland':'爱尔兰',
			    'Iran':'伊朗',
			    'Iraq':'伊拉克',
			    'Iceland':'冰岛',
			    'Israel':'以色列',
			    'Italy':'意大利',
			    'Jamaica':'牙买加',
			    'Jordan':'约旦',
			    'Japan':'日本',
			    'Kazakhstan':'哈萨克斯坦',
			    'Kenya':'肯尼亚',
			    'Kyrgyzstan':'吉尔吉斯斯坦',
			    'Cambodia':'柬埔寨',
			    'South Korea':'韩国',
			    'Kosovo':'科索沃',
			    'Kuwait':'科威特',
			    'Laos':'老挝',
			    'Lebanon':'黎巴嫩',
			    'Liberia':'利比里亚',
			    'Libya':'利比亚',
			    'Sri Lanka':'斯里兰卡',
			    'Lesotho':'莱索托',
			    'Lithuania':'立陶宛',
			    'Luxembourg':'卢森堡',
			    'Latvia':'拉脱维亚',
			    'Morocco':'摩洛哥',
			    'Moldova':'摩尔多瓦',
			    'Madagascar':'马达加斯加',
			    'Mexico':'墨西哥',
			    'Macedonia':'马其顿',
			    'Mali':'马里',
			    'Myanmar':'缅甸',
			    'Montenegro':'黑山',
			    'Mongolia':'蒙古',
			    'Mozambique':'莫桑比克',
			    'Mauritania':'毛里塔尼亚',
			    'Malawi':'马拉维',
			    'Malaysia':'马来西亚',
			    'Namibia':'纳米比亚',
			    'New Caledonia':'新喀里多尼亚',
			    'Niger':'尼日尔',
			    'Nigeria':'尼日利亚',
			    'Nicaragua':'尼加拉瓜',
			    'Netherlands':'荷兰',
			    'Norway':'挪威',
			    'Nepal':'尼泊尔',
			    'New Zealand':'新西兰',
			    'Oman':'阿曼',
			    'Pakistan':'巴基斯坦',
			    'Panama':'巴拿马',
			    'Peru':'秘鲁',
			    'Philippines':'菲律宾',
			    'Papua New Guinea':'巴布亚新几内亚',
			    'Poland':'波兰',
			    'Puerto Rico':'波多黎各',
			    'North Korea':'北朝鲜',
			    'Portugal':'葡萄牙',
			    'Paraguay':'巴拉圭',
			    'Qatar':'卡塔尔',
			    'Romania':'罗马尼亚',
			    'Russia':'俄罗斯',
			    'Rwanda':'卢旺达',
			    'Western Sahara':'西撒哈拉',
			    'Saudi Arabia':'沙特阿拉伯',
			    'Sudan':'苏丹',
			    'South Sudan':'南苏丹',
			    'Senegal':'塞内加尔',
			    'Solomon Islands':'所罗门群岛',
			    'Sierra Leone':'塞拉利昂',
			    'El Salvador':'萨尔瓦多',
			    'Somaliland':'索马里兰',
			    'Somalia':'索马里',
			    'Republic of Serbia':'塞尔维亚',
			    'Suriname':'苏里南',
			    'Slovakia':'斯洛伐克',
			    'Slovenia':'斯洛文尼亚',
			    'Sweden':'瑞典',
			    'Swaziland':'斯威士兰',
			    'Syria':'叙利亚',
			    'Chad':'乍得',
			    'Togo':'多哥',
			    'Thailand':'泰国',
			    'Tajikistan':'塔吉克斯坦',
			    'Turkmenistan':'土库曼斯坦',
			    'East Timor':'东帝汶',
			    'Trinidad and Tobago':'特里尼达和多巴哥',
			    'Tunisia':'突尼斯',
			    'Turkey':'土耳其',
			    'United Republic of Tanzania':'坦桑尼亚',
			    'Uganda':'乌干达',
			    'Ukraine':'乌克兰',
			    'Uruguay':'乌拉圭',
			    'United States of America':'美国',
			    'Uzbekistan':'乌兹别克斯坦',
			    'Venezuela':'委内瑞拉',
			    'Vietnam':'越南',
			    'Vanuatu':'瓦努阿图',
			    'West Bank':'西岸',
			    'Yemen':'也门',
			    'South Africa':'南非',
			    'Zambia':'赞比亚',
			    'Zimbabwe':'津巴布韦'
			};

		map_char_activition = echarts.init(document.getElementById('_add_map'));
		requestActivationMapData("activation",7,option2,map_char_activition,userType,userStatus,values_activation);

		option2 = {
			    timeline: {
			        axisType: 'category',
			            orient: 'vertical',
			            autoPlay: true,
			            inverse: true,
			            playInterval: 5000,
			            left: null,
			            right: -105,
			            top: 20,
			            bottom: 20,
			            width: 46,
			        data: ['2016',]  
			    },
			    baseOption: {
			       visualMap: {
			            max: 100,
			            calculable: true,
			            inRange: {
			                color: ['#ffffbf', '#fee090', '#fdae61', '#f46d43', '#d73027', '#a50026']
			            }
			        },
			        series: [{
			            type: 'map',
			            map: 'world',
			            roam: true,
			            nameMap: nameMap
			        }]
			    },
			    options: [{
			        title: {
			           
			        },
			        series: {
			            data: value3
			        } 
			    }, ]
			};

		function requestActivationMapData(type,date,options,ec,userType,userStatus,values){
			var requestUrl = "${baseURL}/region/Map/"+type+"/"+date+"/"+userType+"/"+userStatus;
			ec.showLoading({
				text:'数据加载中...'
			});
			$.ajax({
				url:requestUrl,  
		        type:'get',  
		        dataType:'json',  
		        success:function(jsons){ 
		        	ec.hideLoading();

		            var Item = function(){  
		                return {  
		                    name:'',  
		                    value:0 
		                    }  
		                };  
		           var Series = []; 
		           var data1 =[];
		           
		            var json = jsons.data; 

		            for(var i=0;i < json.length;i++){  
		                var it = new Item();  
		                it.name = json[i].country;  
		                it.value = json[i].count;  
		                values.push(it);  
		            }
		       
		           console.log(values);
		         	optionk={
					    timeline: {
					        axisType: 'category',
					            orient: 'vertical',
					            autoPlay: true,
					            inverse: true,
					            playInterval: 5000,
					            left: null,
					            right: -105,
					            top: 20,
					            bottom: 20,
					            width: 46,
					        data: ['2016',]  
					    },
					    baseOption: {
					       visualMap: {
					            max: 10000,
					            calculable: true,
					            inRange: {
					                color: ['#ffffbf', '#fee090', '#fdae61', '#f46d43', '#d73027', '#a50026']
					            }
					        },
					        series: [{
					            type: 'map',
					            map: 'world',
					            roam: true,
					            nameMap: nameMap
					        }]
					    },
					    options: [{
					        title: {
					            
					        },
					        series: {
					            data: values
					        } 
					    }, ]
					};
		         	ec.setOption(optionk);
		            
		            //ec.clear(); 
		          //  ec.setOption(options);// 重新加载图表  
		            //values=[];
		            window.onresize = ec.resize;
		        },  
		        error:function(){  
		        		ec.hideLoading();

		           console.log("show requestData error");
		           // alert("数据加载失败！请检查数据链接是否正确");  
		        }  
			});
		}
返回数据
	
	{data: [{country: "中国", dayActive: 0, occupancy: "100.0%", count: 1, sum: 0}]}

###折线图
h5
	
	<div id="_model_line" style="height:500px;padding-top: 10px;"></div>

js

		model_option = {
				tooltip: {
					trigger: 'axis'
				},
				legend: {
					data: []
				},
				toolbox: {
					show: true,
					feature: {
						mark: {
							show: true
						},
						dataView: {
							show: true,
							readOnly: false
						},
						magicType: {
							show: true,
							type: ['line', 'bar', 'stack', 'tiled']
						},
						restore: {
							show: true
						},
						saveAsImage: {
							show: true
						}
					}
				},
返回数据
	
	{"data":[{"sn":null,"dev_name":null,"country":null,"mobile_op":null,"mobile_model":null,"mobile_brand":"test","hour":null,"count":0,"dayActive":0,"sum":1,"occupancy":"100.0%"}]}


