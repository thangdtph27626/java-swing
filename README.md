# Hướng dẫn CRUD list fix cứng trên giao diện SWING

CRUD (Create, Read, Update, Delete) là các nguyên tắc cơ bản và xương sống cơ bản của bất kỳ hệ thống cơ sở dữ liệu SQL nào. CRUD thường được sử dụng trong cơ sở dữ liệu và các trường hợp thiết kế cơ sở dữ liệu. Nó đơn giản hóa việc kiểm soát bảo mật bằng cách đáp ứng nhiều tiêu chí truy cập khác nhau. Từ viết tắt CRUD xác định tất cả các chức năng chính vốn có của cơ sở dữ liệu quan hệ và các ứng dụng được sử dụng để quản lý chúng, bao gồm Cơ sở dữ liệu Oracle , Microsoft SQL Server, MySQL, PostgreSQL và các chức năng khác. Trong bài viết này, chúng ta sẽ học cách thực hiện các thao tác CRUD bằng list fix cứng trên giao diện SWING.

Giả sử ta giải quyết bài toán trên 1 bảng có cấu trúc Book(String maSach, String tenSach, String theLoai, Double gia)
Thông tin lớp học được biểu diễn thành class sau:

```

package com.thang.model;

import java.io.Serializable;


public class Book implements Serializable{
    private String maSach;
    private String tenSach;
    private String theLoai;
    private Double gia;

    public Book() {
    }

    public Book(String maSach, String tenSach, String theLoai, Double gia) {
        this.maSach = maSach;
        this.tenSach = tenSach;
        this.theLoai = theLoai;
        this.gia = gia;
    }

    public String getMaSach() {
        return maSach;
    }

    public void setMaSach(String maSach) {
        this.maSach = maSach;
    }

    public String getTenSach() {
        return tenSach;
    }

    public void setTenSach(String tenSach) {
        this.tenSach = tenSach;
    }

    public String getTheLoai() {
        return theLoai;
    }

    public void setTheLoai(String theLoai) {
        this.theLoai = theLoai;
    }

    public Double getGia() {
        return gia;
    }

    public void setGia(Double gia) {
        this.gia = gia;
    }
}

```

Khởi tạo giao diện swing 

chọn Jframe form 
![image](https://github.com/thangdtph27626/java-swing/assets/109157942/5f0aab3b-6975-44ee-93d7-97db9b9835e8)

chọn tab design 

![image](https://github.com/thangdtph27626/java-swing/assets/109157942/226ef443-7ac1-4fbb-bf4d-8219ce614282)

bạn chọn tab source để thực hiện các thao tác thêm sửa xóa 

```
package com.thang.main;

import com.thang.file.Xfile;
import com.thang.model.Book;
import java.io.IOException;
import java.util.ArrayList;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JOptionPane;
import javax.swing.table.DefaultTableModel;

public class Main extends javax.swing.JFrame {

    private DefaultTableModel model;
    private ArrayList<Book> list = new ArrayList<>();
    private int index = 0;
    
    public Main() {
        initComponents();
        initData();
        initTable();
        //filltable();
        lbIndex.setText(index + "/" + list.size());
    }

// khởi tạo một list sách 
    private void initData() {
        list.add(new Book("PH1", "java1", "hoc tap", 10000.0));
        list.add(new Book("PH2", "java2", "hoc tap", 20000.0));
        list.add(new Book("PH3", "java3", "hoc tap", 30000.0));
    }

// khởi tạo tên colum cho tabletable
    private void initTable() {
        model = new DefaultTableModel();
        model.setColumnIdentifiers(new String[]{"ma sach", "ten", "the loai", "gia"});
        tblBook.setModel(model);
    }

// đổ dữ liệu lên table 
    private void filltable() {
        while (model.getRowCount() > 0) {
            model.setRowCount(0);
        }
        for (Book book : list) {
            Object[] row = {book.getMaSach(), book.getTenSach(), book.getTheLoai(), book.getGia()};
            model.addRow(row);
        }
        model.fireTableDataChanged();
    }

// detail book 
    private void loadTextFile(int index) {
        if (tblBook.getRowCount() > 0) {
            txtID.setText(tblBook.getValueAt(index, 0).toString());
            txtName.setText(tblBook.getValueAt(index, 1).toString());
            txtPrice.setText(tblBook.getValueAt(index, 3).toString());
            txtTheLoai.setText(tblBook.getValueAt(index, 2).toString());
        }
    }

    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">//GEN-BEGIN:initComponents
    private void initComponents() {
      .........
        pack();
        setLocationRelativeTo(null);
    }// </editor-fold>//GEN-END:initComponents

// thêm mới dữ liệu vào list 
    private void btnAddActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnAddActionPerformed
        if (check() && !findID(txtID.getText())) {
            list.add(newBook());
            Xfile.WirterFile(list);
        }
            
       
        filltable();
    }//GEN-LAST:event_btnAddActionPerformed

// xóa dữ liệu 
    private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnDeleteActionPerformed
        if (check()) {
            for (int i = 0; i < list.size(); i++) {
                if (txtID.getText().equalsIgnoreCase(list.get(i).getMaSach())) {
                    list.remove(i);
                    break;
                }
            }
            Xfile.WirterFile(list);
        }
        filltable();
    }//GEN-LAST:event_btnDeleteActionPerformed

    private void btnLoadActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnLoadActionPerformed
        try {
            list = (ArrayList<Book>) Xfile.ReadFile();
            filltable();
            loadTextFile(0);
            tblBook.setRowSelectionInterval(0, 0);
        } catch (IOException ex) {

        }
    }//GEN-LAST:event_btnLoadActionPerformed

// Tạo sự kiện trên table 
    private void tblBookMouseClicked(java.awt.event.MouseEvent evt) {//GEN-FIRST:event_tblBookMouseClicked
        int index = tblBook.getSelectedRow();
        loadTextFile(index);
    }//GEN-LAST:event_tblBookMouseClicked
\
// update dữ liệu 
    private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnUpdateActionPerformed
        if (check()) {
            String id = JOptionPane.showInputDialog(this, "vui lòng nhạp id");
            for (int i = 0; i < list.size(); i++) {
                if (id.equalsIgnoreCase(list.get(i).getMaSach())) {
                    list.set(i, newBook());
                    break;
                }
            }
            Xfile.WirterFile(list);
        }
        filltable();
    }//GEN-LAST:event_btnUpdateActionPerformed

    private void btnfirstActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnfirstActionPerformed
        index = 0;
        tblBook.setRowSelectionInterval(index, index);
        lbIndex.setText((index +1)+ "/" + list.size());
        loadTextFile(index);
    }//GEN-LAST:event_btnfirstActionPerformed

    private void btnLastActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnLastActionPerformed
        index = list.size() - 1;
        tblBook.setRowSelectionInterval(index, index);
        lbIndex.setText((index +1)+ "/" + list.size());
        loadTextFile(index);
    }//GEN-LAST:event_btnLastActionPerformed

    private void btnPreviousActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnPreviousActionPerformed
        if (index <= list.size() - 1 && index > 0) {
            index--;
        } else {
            index = list.size() - 1;
        }
        tblBook.setRowSelectionInterval(index, index);
        lbIndex.setText((index +1)+ "/" + list.size());
        loadTextFile(index);
    }//GEN-LAST:event_btnPreviousActionPerformed

    private void btnNextActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_btnNextActionPerformed
        if (index < list.size() - 1) {
            index++;
        } else if (index == list.size() - 1) {
            index = 0;
        }
        tblBook.setRowSelectionInterval(index, index);
        lbIndex.setText((index +1)+ "/" + list.size());
        loadTextFile(index);
    }//GEN-LAST:event_btnNextActionPerformed

   
    private boolean check() {
        StringBuffer sb = new StringBuffer();
        int count = 0;
        if (txtID.getText().isEmpty()) {
            sb.append("vui long nhap ma sach");
            count++;
        }
        if (txtName.getText().isEmpty()) {
            sb.append("vui long nhap ten sach");
            count++;
        }
        if (txtTheLoai.getText().isEmpty()) {
            sb.append("vui long nhap the loai sach");
            count++;
        }
        if (txtPrice.getText().isEmpty()) {
            sb.append("vui long nhap gia sach");
            count++;
        } else if (!checkSlary()) {
            sb.append("vui long nhap so cho gia tien");
             count++;
        }
        if (count > 0) {
            JOptionPane.showMessageDialog(this, sb.toString(),"error",JOptionPane.OK_OPTION);
            return false;
        }
        return true;
    }


// detail book 
    private boolean findID(String id){
        for(Book book : list){
            if(book.getMaSach().equals(id)){
                JOptionPane.showMessageDialog(this, "id da ton tai","erro",JOptionPane.OK_OPTION);
                return true;
            }
        }
        return false;
    }
    public boolean checkSlary() {
        try {
            double number = Double.parseDouble(txtPrice.getText());

        } catch (Exception e) {

            return false;
        }
        return true;
    }
    private Book newBook() {
        Book book = new Book();
        book.setMaSach(txtID.getText());
        book.setTenSach(txtName.getText());
        book.setTheLoai(txtTheLoai.getText());
        book.setGia(Double.valueOf(txtPrice.getText()));
        return book;
    }

    public static void main(String args[]) {

        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new Main().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify//GEN-BEGIN:variables
    private javax.swing.JButton btnAdd;
    private javax.swing.JButton btnDelete;
    private javax.swing.JButton btnLast;
    private javax.swing.JButton btnLoad;
    private javax.swing.JButton btnNext;
    private javax.swing.JButton btnPrevious;
    private javax.swing.JButton btnUpdate;
    private javax.swing.JButton btnWriter;
    private javax.swing.JButton btnfirst;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    private javax.swing.JLabel jLabel3;
    private javax.swing.JLabel jLabel4;
    private javax.swing.JLabel jLabel5;
    private javax.swing.JPanel jPanel1;
    private javax.swing.JPanel jPanel2;
    private javax.swing.JScrollPane jScrollPane1;
    private javax.swing.JLabel lbIndex;
    private javax.swing.JTable tblBook;
    private javax.swing.JTextField txtID;
    private javax.swing.JTextField txtName;
    private javax.swing.JTextField txtPrice;
    private javax.swing.JTextField txtTheLoai;
    // End of variables declaration//GEN-END:variables
}

```

Bạn có thể tham khảo cách tách ra file service [tại đây](https://github.com/HangNT169/MOB1023_IT17304_BL1_SUM22/blob/master/src/B7_BaiMauCURD_TachService/ViewSinhVien.java)
