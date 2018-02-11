
pom.xml导入

	<!-- 操作Excel工具包 start -->
		<dependency>
			<groupId>net.sourceforge.jexcelapi</groupId>
			<artifactId>jxl</artifactId>
			<version>2.6.12</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>3.16</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>3.16</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml-schemas</artifactId>
			<version>3.16</version>
		</dependency>
	<!-- 操作Excel工具包 End -->


ExcelHelper
	
	/**
	 * Excel工具抽象类
	 * @author w77996
	 * @date 2017-7-4
	 */
	public abstract class ExcelHelper {

	/**
	 * 对象序列化版本号名称
	 */
	public static final String UID = "serialVersionUID";
	
	/**
	 * 将指定excel文件中的数据转换成数据列表
	 * 
	 * @param clazz
	 *            数据类型
	 * @param sheetNo
	 *            工作表编号
	 * @param hasTitle
	 *            是否带有标题
	 * @return 返回转换后的数据列表
	 * @throws Exception
	 */
	public <T> List<T> readExcel(Class<T> clazz, int sheetNo, boolean hasTitle)
			throws Exception {
		Field[] fields = clazz.getDeclaredFields();
		String[] fieldNames = new String[fields.length];
		for (int i = 0; i < fields.length; i++) {
			String fieldName = fields[i].getName();
			fieldNames[i] = fieldName;
		}
		return readExcel(clazz, fieldNames, sheetNo, hasTitle);
	}

	/**
	 * 将指定excel文件中的数据转换成数据列表
	 * 
	 * @param clazz
	 *            数据类型
	 * @param fieldNames
	 *            属性列表
	 * @param hasTitle
	 *            是否带有标题
	 * @return 返回转换后的数据列表
	 * @throws Exception
	 */
	public <T> List<T> readExcel(Class<T> clazz, String[] fieldNames,
			boolean hasTitle) throws Exception {
		return readExcel(clazz, fieldNames, 0, hasTitle);
	}

	/**
	 * 抽象方法：将指定excel文件中的数据转换成数据列表，由子类实现
	 * 
	 * @param clazz
	 *            数据类型
	 * @param fieldNames
	 *            属性列表
	 * @param sheetNo
	 *            工作表编号
	 * @param hasTitle
	 *            是否带有标题
	 * @return 返回转换后的数据列表
	 * @throws Exception
	 */
	public abstract <T> List<T> readExcel(Class<T> clazz, String[] fieldNames,
			int sheetNo, boolean hasTitle) throws Exception;

	/**
	 * 写入数据到指定excel文件中
	 * 
	 * @param clazz
	 *            数据类型
	 * @param dataModels
	 *            数据列表
	 * @throws Exception
	 */
	public <T> void writeExcel(Class<T> clazz, List<T> dataModels)
			throws Exception {
		Field[] fields = clazz.getDeclaredFields();
		String[] fieldNames = new String[fields.length];
		for (int i = 0; i < fields.length; i++) {
			String fieldName = fields[i].getName();
			fieldNames[i] = fieldName;
		}
		writeExcel(clazz, dataModels, fieldNames, fieldNames);
	}

	/**
	 * 写入数据到指定excel文件中
	 * 
	 * @param clazz
	 *            数据类型
	 * @param dataModels
	 *            数据列表
	 * @param fieldNames
	 *            属性列表
	 * @throws Exception
	 */
	public <T> void writeExcel(Class<T> clazz, List<T> dataModels,
			String[] fieldNames) throws Exception {
		writeExcel(clazz, dataModels, fieldNames, fieldNames);
	}

	/**
	 * 抽象方法：写入数据到指定excel文件中，由子类实现
	 * 
	 * @param clazz
	 *            数据类型
	 * @param dataModels
	 *            数据列表
	 * @param fieldNames
	 *            属性列表
	 * @param titles
	 *            标题列表
	 * @throws Exception
	 */
	public abstract <T> void writeExcel(Class<T> clazz, List<T> dataModels,
			String[] fieldNames, String[] titles) throws Exception;

	/**
	 * 判断属性是否为日期类型
	 * 
	 * @param clazz
	 *            数据类型
	 * @param fieldName
	 *            属性名
	 * @return 如果为日期类型返回true，否则返回false
	 */
	protected <T> boolean isDateType(Class<T> clazz, String fieldName) {
		boolean flag = false;
		try {
			Field field = clazz.getDeclaredField(fieldName);
			Object typeObj = field.getType().newInstance();
			flag = typeObj instanceof Date;
		} catch (Exception e) {
			// 把异常吞掉直接返回false
		}
		return flag;
	}

	/**
	 * 根据类型将指定参数转换成对应的类型
	 * 
	 * @param value
	 *            指定参数
	 * @param type
	 *            指定类型
	 * @return 返回类型转换后的对象
	 */
	protected <T> Object parseValueWithType(String value, Class<?> type) {
		Object result = null;
		try { // 根据属性的类型将内容转换成对应的类型
			if (Boolean.TYPE == type) {
				result = Boolean.parseBoolean(value);
			} else if (Byte.TYPE == type) {
				result = Byte.parseByte(value);
			} else if (Short.TYPE == type) {
				result = Short.parseShort(value);
			} else if (Integer.TYPE == type) {
				result = Integer.parseInt(value);
			} else if (Long.TYPE == type) {
				result = Long.parseLong(value);
			} else if (Float.TYPE == type) {
				result = Float.parseFloat(value);
			} else if (Double.TYPE == type) {
				result = Double.parseDouble(value);
			} else {
				result = (Object) value;
			}
		} catch (Exception e) {
			// 把异常吞掉直接返回null
		}
		return result;
	}

	public static void main(String[] args) {
		List<Employee> employees = new ArrayList<Employee>();
		employees.add(new Employee(1000, "Jones", 40, "Manager", 2975));
		employees.add(new Employee(1001, "Blake", 40, "Manager", 2850));
		employees.add(new Employee(1002, "Clark", 40, "Manager", 2450));
		employees.add(new Employee(1003, "Scott", 30, "Analyst", 3000));
		employees.add(new Employee(1004, "King", 50, "President", 5000));
		String[] titles = new String[]{"工号", "姓名", "年龄", "职称", "薪资（美元）", "入职时间"};
		String[] fieldNames = new String[]{"id", "name", "age", "job", "salery", "addtime"};
		try {
			File file1 = new File("E:\\JXL2003.xls");
			ExcelHelper eh1 = JxlExcelHelper.getInstance(file1);
			eh1.writeExcel(Employee.class, employees);
			eh1.writeExcel(Employee.class, employees, fieldNames, titles);
			List<Employee> list1 = eh1.readExcel(Employee.class, fieldNames, true);
			System.out.println("-----------------JXL2003.xls-----------------");
			for (Employee user : list1) {
				System.out.println(user);
			}
			File file2 = new File("E:\\POI2003.xls");
			ExcelHelper eh2 = HssfExcelHelper.getInstance(file2);
			eh2.writeExcel(Employee.class, employees);
			eh2.writeExcel(Employee.class, employees, fieldNames, titles);
			List<Employee> list2 = eh2.readExcel(Employee.class, fieldNames, true);
			System.out.println("-----------------POI2003.xls-----------------");
			for (Employee user : list2) {
				System.out.println(user);
			}
			File file3 = new File("E:\\POI2007.xlsx");
			ExcelHelper eh3 = XssfExcelHelper.getInstance(file3);
			eh3.writeExcel(Employee.class, employees);
			eh3.writeExcel(Employee.class, employees, fieldNames, titles);
			List<Employee> list3 = eh3.readExcel(Employee.class, fieldNames, true);
			System.out.println("-----------------POI2007.xls-----------------");
			for (Employee user : list3) {
				System.out.println(user);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	}


HssfExcelHelper
	
	/**
	 * 基于POI实现Excel工具类
	 * @author w77996
	 * @date 2017-7-4
	 */
	public class HssfExcelHelper extends ExcelHelper {

	private static HssfExcelHelper instance = null; // 单例对象

	private File file; // 操作文件

	/**
	 * 私有化构造方法
	 * 
	 * @param file
	 *            文件对象
	 */
	private HssfExcelHelper(File file) {
		super();
		this.file = file;
	}

	public File getFile() {
		return file;
	}

	public void setFile(File file) {
		this.file = file;
	}

	/**
	 * 获取单例对象并进行初始化
	 * 
	 * @param file
	 *            文件对象
	 * @return 返回初始化后的单例对象
	 */
	public static HssfExcelHelper getInstance(File file) {
		if (instance == null) {
			// 当单例对象为null时进入同步代码块
			synchronized (HssfExcelHelper.class) {
				// 再次判断单例对象是否为null，防止多线程访问时多次生成对象
				if (instance == null) {
					instance = new HssfExcelHelper(file);
				}
			}
		} else {
			// 如果操作的文件对象不同，则重置文件对象
			if (!file.equals(instance.getFile())) {
				instance.setFile(file);
			}
		}
		return instance;
	}

	/**
	 * 获取单例对象并进行初始化
	 * 
	 * @param filePath
	 *            文件路径
	 * @return 返回初始化后的单例对象
	 */
	public static HssfExcelHelper getInstance(String filePath) {
		return getInstance(new File(filePath));
	}

	public <T> List<T> readExcel(Class<T> clazz, String[] fieldNames,
			int sheetNo, boolean hasTitle) throws Exception {
		List<T> dataModels = new ArrayList<T>();
		// 获取excel工作簿
		HSSFWorkbook workbook = new HSSFWorkbook(new FileInputStream(file));
		HSSFSheet sheet = workbook.getSheetAt(sheetNo);
		int start = sheet.getFirstRowNum() + (hasTitle ? 1 : 0); // 如果有标题则从第二行开始
		for (int i = start; i <= sheet.getLastRowNum(); i++) {
			HSSFRow row = sheet.getRow(i);
			if (row == null) {
				continue;
			}
			// 生成实例并通过反射调用setter方法
			T target = clazz.newInstance();
			for (int j = 0; j < fieldNames.length; j++) {
				String fieldName = fieldNames[j];
				if (fieldName == null || UID.equals(fieldName)) {
					continue; // 过滤serialVersionUID属性
				}
				// 获取excel单元格的内容
				HSSFCell cell = row.getCell(j);
				if (cell == null) {
					continue;
				}
				String content = cell.getStringCellValue();
				// 如果属性是日期类型则将内容转换成日期对象
				if (isDateType(clazz, fieldName)) {
					// 如果属性是日期类型则将内容转换成日期对象
					ReflectUtil.invokeSetter(target, fieldName,
							DateUtil.parse(content));
				} else {
					Field field = clazz.getDeclaredField(fieldName);
					ReflectUtil.invokeSetter(target, fieldName,
							parseValueWithType(content, field.getType()));
				}
			}
			dataModels.add(target);
		}
		return dataModels;
	}

	public <T> void writeExcel(Class<T> clazz, List<T> dataModels,
			String[] fieldNames, String[] titles) throws Exception {
		HSSFWorkbook workbook = null;
		// 检测文件是否存在，如果存在则修改文件，否则创建文件
		if (file.exists()) {
			FileInputStream fis = new FileInputStream(file);
			workbook = new HSSFWorkbook(fis);
		} else {
			workbook = new HSSFWorkbook();
		}
		// 根据当前工作表数量创建相应编号的工作表
		String sheetName = DateUtil.format(new Date(), "yyyyMMddHHmmssSS");
		HSSFSheet sheet = workbook.createSheet(sheetName);
		HSSFRow headRow = sheet.createRow(0);
		// 添加表格标题
		for (int i = 0; i < titles.length; i++) {
			HSSFCell cell = headRow.createCell(i);
			cell.setCellType(HSSFCell.CELL_TYPE_STRING);
			cell.setCellValue(titles[i]);
			// 设置字体加粗
			HSSFCellStyle cellStyle = workbook.createCellStyle();
			HSSFFont font = workbook.createFont();
			font.setBoldweight(Font.BOLDWEIGHT_BOLD);
			cellStyle.setFont(font);
			// 设置自动换行
			cellStyle.setWrapText(true);
			cell.setCellStyle(cellStyle);
			// 设置单元格宽度
			sheet.setColumnWidth(i, titles[i].length() * 1000);
		}
		// 添加表格内容
		for (int i = 0; i < dataModels.size(); i++) {
			HSSFRow row = sheet.createRow(i + 1);
			// 遍历属性列表
			for (int j = 0; j < fieldNames.length; j++) {
				// 通过反射获取属性的值域
				String fieldName = fieldNames[j];
				if (fieldName == null || UID.equals(fieldName)) {
					continue; // 过滤serialVersionUID属性
				}
				Object result = ReflectUtil.invokeGetter(dataModels.get(i),
						fieldName);
				HSSFCell cell = row.createCell(j);
				cell.setCellValue(StringUtil.toString(result));
				// 如果是日期类型则进行格式化处理
				if (isDateType(clazz, fieldName)) {
					cell.setCellValue(DateUtil.format((Date) result));
				}
			}
		}
		// 将数据写到磁盘上
		FileOutputStream fos = new FileOutputStream(file);
		try {
			workbook.write(new FileOutputStream(file));
		} finally {
			if (fos != null) {
				fos.close(); // 不管是否有异常发生都关闭文件输出流
			}
		}
	}
	}

JxlExcelHelper

		/**
	 * 基于JXL实现的Excel工具类
	 * @author w77996
	 * @date 2017-7-4
	 */
	public class JxlExcelHelper extends ExcelHelper {

		private static JxlExcelHelper instance = null; // 单例对象
	
		private File file; // 操作文件
	
		/**
		 * 私有化构造方法
		 * 
		 * @param file
		 *            文件对象
		 */
		private JxlExcelHelper(File file) {
			super();
			this.file = file;
		}
	
		public File getFile() {
			return file;
		}
	
		public void setFile(File file) {
			this.file = file;
		}
	
		/**
		 * 获取单例对象并进行初始化
		 * 
		 * @param file
		 *            文件对象
		 * @return 返回初始化后的单例对象
		 */
		public static JxlExcelHelper getInstance(File file) {
			if (instance == null) {
				// 当单例对象为null时进入同步代码块
				synchronized (JxlExcelHelper.class) {
					// 再次判断单例对象是否为null，防止多线程访问时多次生成对象
					if (instance == null) {
						instance = new JxlExcelHelper(file);
					}
				}
			} else {
				// 如果操作的文件对象不同，则重置文件对象
				if (!file.equals(instance.getFile())) {
					instance.setFile(file);
				}
			}
			return instance;
		}
	
		/**
		 * 获取单例对象并进行初始化
		 * 
		 * @param filePath
		 *            文件路径
		 * @return 返回初始化后的单例对象
		 */
		public static JxlExcelHelper getInstance(String filePath) {
			return getInstance(new File(filePath));
		}
	
		public <T> List<T> readExcel(Class<T> clazz, String[] fieldNames,
				int sheetNo, boolean hasTitle) throws Exception {
			List<T> dataModels = new ArrayList<T>();
			// 获取excel工作簿
			Workbook workbook = Workbook.getWorkbook(file);
			try {
				Sheet sheet = workbook.getSheet(sheetNo);
				int start = hasTitle ? 1 : 0; // 如果有标题则从第二行开始
				for (int i = start; i < sheet.getRows(); i++) {
					// 生成实例并通过反射调用setter方法
					T target = clazz.newInstance();
					for (int j = 0; j < fieldNames.length; j++) {
						String fieldName = fieldNames[j];
						if (fieldName == null || UID.equals(fieldName)) {
							continue; // 过滤serialVersionUID属性
						}
						// 获取excel单元格的内容
						Cell cell = sheet.getCell(j, i);
						if (cell == null) {
							continue;
						}
						String content = cell.getContents();
						// 如果属性是日期类型则将内容转换成日期对象
						if (isDateType(clazz, fieldName)) {
							// 如果属性是日期类型则将内容转换成日期对象
							ReflectUtil.invokeSetter(target, fieldName,
									DateUtil.parse(content));
						} else {
							Field field = clazz.getDeclaredField(fieldName);
							ReflectUtil.invokeSetter(target, fieldName,
									parseValueWithType(content, field.getType()));
						}
					}
					dataModels.add(target);
				}
			} finally {
				if (workbook != null) {
					workbook.close();
				}
			}
			return dataModels;
		}
	
		public <T> void writeExcel(Class<T> clazz, List<T> dataModels,
				String[] fieldNames, String[] titles) throws Exception {
			WritableWorkbook workbook = null;
			try {
				// 检测文件是否存在，如果存在则修改文件，否则创建文件
				if (file.exists()) {
					Workbook book = Workbook.getWorkbook(file);
					workbook = Workbook.createWorkbook(file, book);
				} else {
					workbook = Workbook.createWorkbook(file);
				}
				// 根据当前工作表数量创建相应编号的工作表
				int sheetNo = workbook.getNumberOfSheets() + 1;
				String sheetName = DateUtil.format(new Date(), "yyyyMMddHHmmssSS");
				WritableSheet sheet = workbook.createSheet(sheetName, sheetNo);
				// 添加表格标题
				for (int i = 0; i < titles.length; i++) {
					// 设置字体加粗
					WritableFont font = new WritableFont(WritableFont.ARIAL, 10,
							WritableFont.BOLD);
					WritableCellFormat format = new WritableCellFormat(font);
					// 设置自动换行
					format.setWrap(true);
					Label label = new Label(i, 0, titles[i], format);
					sheet.addCell(label);
					// 设置单元格宽度
					sheet.setColumnView(i, titles[i].length() + 10);
				}
				// 添加表格内容
				for (int i = 0; i < dataModels.size(); i++) {
					// 遍历属性列表
					for (int j = 0; j < fieldNames.length; j++) {
						T target = dataModels.get(i);
						// 通过反射获取属性的值域
						String fieldName = fieldNames[j];
						if (fieldName == null || UID.equals(fieldName)) {
							continue; // 过滤serialVersionUID属性
						}
						Object result = ReflectUtil.invokeGetter(target, fieldName);
						Label label = new Label(j, i + 1,
								StringUtil.toString(result));
						// 如果是日期类型则进行格式化处理
						if (isDateType(clazz, fieldName)) {
							label.setString(DateUtil.format((Date) result));
						}
						sheet.addCell(label);
					}
				}
			} finally {
				if (workbook != null) {
					workbook.write();
					workbook.close();
				}
			}
		}
	}

XssfExcelHelper

	public class XssfExcelHelper extends ExcelHelper {

		private static XssfExcelHelper instance = null; // 单例对象
	
		private File file; // 操作文件
	
		/**
		 * 私有化构造方法
		 * 
		 * @param file
		 *            文件对象
		 */
		private XssfExcelHelper(File file) {
			super();
			this.file = file;
		}
	
		public File getFile() {
			return file;
		}
	
		public void setFile(File file) {
			this.file = file;
		}
	
		/**
		 * 获取单例对象并进行初始化
		 * 
		 * @param file
		 *            文件对象
		 * @return 返回初始化后的单例对象
		 */
		public static XssfExcelHelper getInstance(File file) {
			if (instance == null) {
				// 当单例对象为null时进入同步代码块
				synchronized (XssfExcelHelper.class) {
					// 再次判断单例对象是否为null，防止多线程访问时多次生成对象
					if (instance == null) {
						instance = new XssfExcelHelper(file);
					}
				}
			} else {
				// 如果操作的文件对象不同，则重置文件对象
				if (!file.equals(instance.getFile())) {
					instance.setFile(file);
				}
			}
			return instance;
		}
	
		/**
		 * 获取单例对象并进行初始化
		 * 
		 * @param filePath
		 *            文件路径
		 * @return 返回初始化后的单例对象
		 */
		public static XssfExcelHelper getInstance(String filePath) {
			return getInstance(new File(filePath));
		}
	
		public <T> List<T> readExcel(Class<T> clazz, String[] fieldNames,
				int sheetNo, boolean hasTitle) throws Exception {
			List<T> dataModels = new ArrayList<T>();
			// 获取excel工作簿
			XSSFWorkbook workbook = new XSSFWorkbook(new FileInputStream(file));
			XSSFSheet sheet = workbook.getSheetAt(sheetNo);
			int start = sheet.getFirstRowNum() + (hasTitle ? 1 : 0); // 如果有标题则从第二行开始
			for (int i = start; i <= sheet.getLastRowNum(); i++) {
				XSSFRow row = sheet.getRow(i);
				if (row == null) {
					continue;
				}
				// 生成实例并通过反射调用setter方法
				T target = clazz.newInstance();
				for (int j = 0; j < fieldNames.length; j++) {
					String fieldName = fieldNames[j];
					if (fieldName == null || UID.equals(fieldName)) {
						continue; // 过滤serialVersionUID属性
					}
					// 获取excel单元格的内容
					XSSFCell cell = row.getCell(j);
					if (cell == null) {
						continue;
					}
					String content = getCellContent(cell);
					// 如果属性是日期类型则将内容转换成日期对象
					if (isDateType(clazz, fieldName)) {
						// 如果属性是日期类型则将内容转换成日期对象
						ReflectUtil.invokeSetter(target, fieldName,
								DateUtil.parse(content));
					} else {
						Field field = clazz.getDeclaredField(fieldName);
						ReflectUtil.invokeSetter(target, fieldName,
								parseValueWithType(content, field.getType()));
					}
				}
				dataModels.add(target);
			}
			return dataModels;
		}
	
		public <T> void writeExcel(Class<T> clazz, List<T> dataModels,
				String[] fieldNames, String[] titles) throws Exception {
			XSSFWorkbook workbook = null;
			// 检测文件是否存在，如果存在则修改文件，否则创建文件
			if (file.exists()) {
				FileInputStream fis = new FileInputStream(file);
				workbook = new XSSFWorkbook(fis);
			} else {
				workbook = new XSSFWorkbook();
			}
			// 根据当前工作表数量创建相应编号的工作表
			String sheetName = DateUtil.format(new Date(), "yyyyMMddHHmmssSS");
			XSSFSheet sheet = workbook.createSheet(sheetName);
			XSSFRow headRow = sheet.createRow(0);
			// 添加表格标题
			for (int i = 0; i < titles.length; i++) {
				XSSFCell cell = headRow.createCell(i);
				cell.setCellType(HSSFCell.CELL_TYPE_STRING);
				cell.setCellValue(titles[i]);
				// 设置字体加粗
				XSSFCellStyle cellStyle = workbook.createCellStyle();
				XSSFFont font = workbook.createFont();
				font.setBoldweight(Font.BOLDWEIGHT_BOLD);
				cellStyle.setFont(font);
				// 设置自动换行
				cellStyle.setWrapText(true);
				cell.setCellStyle(cellStyle);
				// 设置单元格宽度
				sheet.setColumnWidth(i, titles[i].length() * 1000);
			}
			// 添加表格内容
			for (int i = 0; i < dataModels.size(); i++) {
				T target = dataModels.get(i);
				XSSFRow row = sheet.createRow(i + 1);
				// 遍历属性列表
				for (int j = 0; j < fieldNames.length; j++) {
					// 通过反射获取属性的值域
					String fieldName = fieldNames[j];
					if (fieldName == null || UID.equals(fieldName)) {
						continue; // 过滤serialVersionUID属性
					}
					Object result = ReflectUtil.invokeGetter(target, fieldName);
					XSSFCell cell = row.createCell(j);
					cell.setCellValue(StringUtil.toString(result));
					// 如果是日期类型则进行格式化处理
					if (isDateType(clazz, fieldName)) {
						cell.setCellValue(DateUtil.format((Date) result));
					}
				}
			}
			// 将数据写到磁盘上
			FileOutputStream fos = new FileOutputStream(file);
			try {
				workbook.write(new FileOutputStream(file));
			} finally {
				if (fos != null) {
					fos.close(); // 不管是否有异常发生都关闭文件输出流
				}
			}
		}
	
		protected <T> Object parseValueWithType(String value, Class<?> type) {
			// 由于Excel2007的numeric类型只返回double型，所以对于类型为整型的属性，要提前对numeric字符串进行转换
			if (Byte.TYPE == type || Short.TYPE == type || Short.TYPE == type
					|| Long.TYPE == type) {
				value = String.valueOf((long) Double.parseDouble(value));
			}
			return super.parseValueWithType(value, type);
		}
	
		/**
		 * 获取单元格的内容
		 * 
		 * @param cell
		 *            单元格
		 * @return 返回单元格内容
		 */
		private String getCellContent(XSSFCell cell) {
			StringBuffer buffer = new StringBuffer();
			switch (cell.getCellType()) {
				case XSSFCell.CELL_TYPE_NUMERIC : // 数字
					buffer.append(cell.getNumericCellValue());
					break;
				case XSSFCell.CELL_TYPE_BOOLEAN : // 布尔
					buffer.append(cell.getBooleanCellValue());
					break;
				case XSSFCell.CELL_TYPE_FORMULA : // 公式
					buffer.append(cell.getCellFormula());
					break;
				case XSSFCell.CELL_TYPE_STRING : // 字符串
					buffer.append(cell.getStringCellValue());
					break;
				case XSSFCell.CELL_TYPE_BLANK : // 空值
				case XSSFCell.CELL_TYPE_ERROR : // 故障
				default :
					break;
			}
			return buffer.toString();
		}
	}

java

	/**
	 * 报表导入
	 */
	@RequestMapping(value = "/import", method = RequestMethod.POST)
	public String importExcel(@RequestParam(value="filename")MultipartFile file,HttpServletRequest request,HttpServletResponse response){
		try {
			//判断是否为空
			if(file == null) return null;
			//获取文件名字
			String name = file.getOriginalFilename();
			//进一步判断文件是否为空（即判断其大小是否为0或其名称是否为null）
			long size = file.getSize();
			if(name == null || ("".equals(name) && size == 0)) return null;
			//获取文件后缀名
			if(name.indexOf(".") == -1) return null;
			String suffix = name.substring(name.lastIndexOf(".")+1);
			if(!("xls".equals(suffix) || "xlsx".equals(suffix))){
				return null;
			}
			String path = request.getSession().getServletContext().getRealPath(ConstantClass.EXCEL_PATH);
			path = path + "/" + new Date().getTime() + "." + suffix;
			File excelFile = new File(path);
			file.transferTo(excelFile);
			String[] fieldNames = new String[]{"company", "department", "model", "sn"};
			WtDeviceInfo wdi = null;
			WtUserInfo wui = getPmsOperator();
			String	dateString = DateUtil.format(new Date(), DateUtil.PATTERN_CLASSICAL);
			Date date = DateUtil.parse(dateString);
			if("xls".equals(suffix)){
				ExcelHelper eh = JxlExcelHelper.getInstance(excelFile);
				List<WtDeviceInfo> list = eh.readExcel(WtDeviceInfo.class, fieldNames, true);
				if(list.size() < 1){
					return null;
				}
				wdi = list.get(0);
				String devName = wdi.getModel();
				for (int i = 0; i< list.size();i++) {
					list.get(i).setCreateTime(date);
				}
				wtDeviceService.insertDeviceIfNoExist(devName);
				wtDeviceInfoService.saveBatchDevice(list,wdi,wui.getUserName());
			}else if("xlsx".equals(suffix)){
				ExcelHelper eh = XssfExcelHelper.getInstance(excelFile);
				List<WtDeviceInfo> list = eh.readExcel(WtDeviceInfo.class, fieldNames, true);
				if(list.size() < 1){
					return null;
				}
				wdi = list.get(0);
				String devName = wdi.getModel();
				for (int i = 0; i< list.size();i++) {
					list.get(i).setCreateTime(date);
				}
				wtDeviceService.insertDeviceIfNoExist(devName);
				wtDeviceInfoService.saveBatchDevice(list,wdi,wui.getUserName());
			}else{
				//请导入后缀名为xls或xlsx的excel
				return null;
			}
		} catch (Exception e) {
			log.error("====DeviceStorageController-->importExcel error:"+e.getMessage());
		}
		return "redirect:/device/storage/list";
	}
	}
