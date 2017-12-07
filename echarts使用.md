
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

js

返回数据

###折线图
h5

js

返回数据