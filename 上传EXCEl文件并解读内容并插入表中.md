# 1.上传 EXCEL 文件
> 前端 <a href="https://element.eleme.cn/#/zh-CN/component/upload"> element-ui</a>
```html
    <el-upload
        class="upload-demo"
        ref="uploadexcel"
        :file-list="fileListUploadexcel"
        :action="uploadUrlExcel"
        :headers="uploadHeaders"
        :auto-upload="false"
        accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
        >
        <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUploadExcel">上传到服务器</el-button>
        <div slot="tip" class="el-upload__tip">只能上传excel文件</div>
    </el-upload>
```
>js部分
```js
import { getApiUrl } from "@/utils/utils";
import { getToken } from "@/utils/auth";
import axios from 'axios'

export default {
  name: "Breed",
  data() {
    return {
      loading: true,
      multiple: true,
      apiUrl: getApiUrl(),
      uploadUrlExcel: "",
      uploadHeaders: {
        Authorization: "",
      },
      fileListUploadexcel:[],
    };
  },
  created() {
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.uploadUrlExcel = getApiUrl() + "/common/upload/excel";
      this.uploadHeaders["Authorization"] = getToken();
      
    },
    submitUploadExcel(){
      this.$refs.uploadexcel.submit();
    }
  }
};
```

>后端java 接收文件并解读,存库
- 需要的依赖
```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>4.0.1</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>4.0.1</version>
</dependency>
<dependency>
    <groupId>org.apache.xmlbeans</groupId>
    <artifactId>xmlbeans</artifactId>
    <version>3.0.2</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>ooxml-schemas</artifactId>
    <version>1.1</version>
</dependency>
```
- java code
```java
 @PostMapping("/common/upload/excel")
public void uploadFileExcels(MultipartFile file) throws Exception
{
    try
    {
        //获取文件的流
        InputStream input = file.getInputStream();
        //解析Excel
        Workbook wb = WorkbookFactory.create(input);
        Sheet sheet = wb.getSheetAt(0);
        System.out.println("导入数据条数 === " + sheet.getLastRowNum());
        //跳过第一行为表头
        for(int i=1;i<=sheet.getLastRowNum();i++) {
            Row row = sheet.getRow(i);
            //数据为空，往下加载
            if(null == row) {
                continue;
            }
            //获取第一列,品种名称
            String nurseryName = String.valueOf(row.getCell(0));
            //获取第二列,品种描述
            String nurseryDescs = String.valueOf(row.getCell(1));
            //从数据库查出所有的品种
            List<SysNursery> nurseryList = sysNurseryService.selectSysNurseryList(null);

            for(SysNursery item : nurseryList){

                if(item.getName().equals(nurseryName)){
                    item.setDescs(nurseryDescs);
                    sysNurseryService.updateSysNursery(item);
                }
            }
        }
    }
    catch (IOException e)
    {
        System.out.println("---文件解析异常---"+e.getMessage());
    }
}
```