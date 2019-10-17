# servlet 中的一些知识点

### 1 实现文件下载

```java
public class Servletresponsedemo3 extends HttpServlet {
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        String realPath = this.getServletContext().getRealPath("/img/1.txt");
        System.out.println(realPath);
        File file = new File(realPath);
        System.out.println(file);
        ServletOutputStream out = response.getOutputStream();
        if (file.exists()){
            response.setHeader("content-type", "application/octet-stream");
            response.setContentType("application/octet-stream");
            String fileName = file.getName();
            response.setHeader("Content-Disposition", "attachment;filename="+new String(fileName.getBytes("utf-8"),"ISO-8859-1"));
            byte[] bytes = new byte[1024];
            FileInputStream fis = null;
            BufferedInputStream bis = null;
            try {
                fis = new FileInputStream(file);
                bis = new BufferedInputStream(fis);
                int length = 0;
                while (length!=-1){
                    length = bis.read(bytes);
                    out.write(bytes,0,length);
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } finally {
                if (bis!=null){
                    bis.close();
                }
                if (fis!=null){
                    fis.close();
                }
                if(out!=null){
                    out.close();
                }
            }

        }

        out.write("文件不存在".getBytes("utf-8"));




    }
```

