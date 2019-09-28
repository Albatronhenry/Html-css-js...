### [jquery+poi前后端实现导出文件](https://blog.csdn.net/Sunny__wei/article/details/70214103)

项目需要将数据导出为excel文件
--------------------------
前端：发现jquery使用ajax调用浏览器无法下载，这是ajax存在的一个问题，解决方法如下：
```js
	viewModel.exportExcel=function(){
			  var datas = {};
			  datas["court_code"] = fyfzxm;
	          postDownLoadFile({
	            url:$ctx + "/Setting/exportExcel",
	            data:datas,
	            method:'post'
	          });
	}
						

	/*===================post请求下载文件
	* options:{
	* url:'',  //下载地址
	* data:{name:value}, //要发送的数据
	* method:'post'
	* }
	*/
	var postDownLoadFile = function (options) {
			var config = $.extend(true, { method: 'post' }, options);
			var $iframe = $('<iframe id="down-file-iframe" />');
			var $form = $('<form target="down-file-iframe" method="' + config.method + '" />');
			$form.attr('action', config.url);
			for (var key in config.data) {
			    $form.append('<input type="hidden" name="' + key + '" value="' + config.data[key] + '" />');
			}
			$iframe.append($form);
			$(document.body).append($iframe);
			$form[0].submit();
			$iframe.remove();
	}

```

后端实现如下：
```java
@SuppressWarnings("deprecation")
	@RequestMapping(value = "exportExcel")
	public void excel(HttpServletRequest request, HttpServletResponse response) {
		String pageIndex = "0";
		String pageSize = "1000000";

		String fyfzxm = request.getParameter("fyfzxm");
		String fymc = request.getParameter("fymc");

		List<CourtInfoEntity> exportLs = commonService.findList(fyfzxm,fymc,pageIndex,pageSize);
		//组装导出文件
		XSSFWorkbook wb = null;
		OutputStream os = null;
		try {
			// excel 构建
			wb = new XSSFWorkbook();
			SimpleDateFormat sdflog = new SimpleDateFormat("yyyy-MM-dd"); 
			String logtime = sdflog.format(new Date());
			logtime = "****虚拟账号使用情况表"+logtime;			
			XSSFSheet sheet = wb.createSheet(logtime);			
			//	标题		
			sheet.addMergedRegion(new CellRangeAddress(0, 0, 0, 5));
			XSSFRow header_row = sheet.createRow(0);
			header_row.createCell(0).setCellValue(logtime);
			// 表头列
			XSSFRow row = sheet.createRow(1);
			// 拼装表头列
			row.createCell(0).setCellValue("单位代码");
			row.createCell(1).setCellValue("法院名称");
			row.createCell(2).setCellValue("开户银行");
			row.createCell(3).setCellValue("虚拟账号预警值");
			row.createCell(4).setCellValue("虚拟账号剩余值");
			row.createCell(5).setCellValue("是否需要获取虚拟账号");
				for (int i = 0, l = exportLs.size(); i < l; i++) {
					CourtInfoEntity dataCell =  exportLs.get(i);
					// 数据列
					XSSFRow _row = sheet.createRow(i + 2);
					_row.createCell(0).setCellValue(dataCell.getFyfzxm());
					_row.createCell(1).setCellValue(dataCell.getFymc());
					_row.createCell(2).setCellValue(dataCell.getYhmc());
					_row.createCell(3).setCellValue(dataCell.getCritical_num());
					_row.createCell(4).setCellValue(dataCell.getAccount());
					_row.createCell(5).setCellValue(dataCell.getIs_need_get_vr());
				}
				// 设置Header并且输出文件
				String fileName = logtime;
				response.setHeader("Content-Disposition","attachment; filename=" + new String((fileName + ".xlsx").getBytes(), "iso-8859-1"));
				response.setContentType("application/vnd.ms-excel");
				response.setCharacterEncoding("UTF-8");
				os = response.getOutputStream();
				wb.write(os);
				os.flush();
		}catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				os.close();
			} catch (IOException e) {
			}
		}				
	}


```
