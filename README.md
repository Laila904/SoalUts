1. Langkat pertama yaitu membuat Databes di postgreSQL dengan menggunakn pgAdmin
 ![image](https://github.com/user-attachments/assets/7555bf1c-aa9a-4ead-be69-8595b1b9c1c2)

2. Langkah kedua menghubungkan database ke java/neTBeans.
![image](https://github.com/user-attachments/assets/0dc71248-51d3-4f2a-bdd5-1d1f32aca07e)

3. Langkah ketiga yaitu membuat  tampilan GUI.
 ![image](https://github.com/user-attachments/assets/6b9d2fca-4c3e-4f64-a781-3bb6c8dbc47f)

4. Langkah keempat yaitu masukan kode insert ditampilan GUI.
   private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {                                          

        if (txtKode.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtSks.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtNama.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtSemester.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO Matakuliah(KodeMK, Sks, NamaMK, Semesterajar) VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                String KodeMK, Sks, NamaMK, Semesterajar;
                KodeMK = txtKode.getText();
                Sks = txtSks.getText();
                NamaMK = txtNama.getText();
                Semesterajar = txtSemester.getText();
                pstmt.setInt(1, Integer.parseInt(KodeMK));
                pstmt.setString(2, Sks);
                pstmt.setString(3, NamaMK);
                pstmt.setString(4, Semesterajar);

                pstmt.executeUpdate();
                conn.commit();

                pstmt.close();
                conn.close();

                JOptionPane.showMessageDialog(null, "Succes to insert");
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Failed to insert " + ex.getMessage());
            }
        }
        bersih();
        showTable();
    }
   Tampilan ketika insert:
   ![image](https://github.com/user-attachments/assets/1c09497c-c84e-4e15-95dd-10d459ed5b63)
                     
6. Langkah keempat yaitu masukan kode Update ditampilan GUI.
   private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        String KodeMK, Sks, NamaMK, Semesterajar;
        if (txtKode.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtSks.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtNama.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtSemester.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE Matakuliah SET Sks = ?, NamaMK = ?, Semesterajar = ? WHERE KodeMk = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                KodeMK = txtKode.getText();
                Sks = txtSks.getText();
                NamaMK = txtNama.getText();
                Semesterajar = txtSemester.getText();

                pstmt.setString(1, Sks);
                pstmt.setString(2, NamaMK);
                pstmt.setString(3, Semesterajar);
                pstmt.setInt(4, Integer.parseInt(KodeMK));

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "update Sukses");
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "Update gagal");
                }
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Error: " + ex.getMessage());
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(null, "ISBN harus berupa angka");
            } finally {
                try {
                    if (pstmt != null) {
                        pstmt.close();
                    }
                    if (conn != null) {
                        conn.close();
                    }
                } catch (SQLException e) {
                    JOptionPane.showMessageDialog(null, "Error closing resources: " + e.getMessage());
                }
            }
        }
        showTable();

    }

Tampilan ketika sebelum diUpdate:
![image](https://github.com/user-attachments/assets/374446fd-5e06-4108-a2ca-98db0df18a3a)

Tampilan Sesudah di update:
![image](https://github.com/user-attachments/assets/a850b97b-653b-4bb5-8554-163aa9a79c98)
                                
8.  Langkah keempat yaitu masukan kode Delete ditampilan GUI.
   private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String KodeMK;
        KodeMK = txtKode.getText();

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Silakan Konfirmasi?");
            switch (jawab) {
                case JOptionPane.YES_OPTION:
                    String deleteSql = "DELETE FROM Matakuliah WHERE KodeMK = ?";
                    pstmt = conn.prepareStatement(deleteSql);
                    pstmt.setInt(1, Integer.parseInt(KodeMK));
                    pstmt.executeUpdate();
                    pstmt.close();
                    conn.close();

                    break;
                case JOptionPane.NO_OPTION:
                    JOptionPane.showMessageDialog(this, "Kamu menjawab tidak");
                    break;
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showConfirmDialog(null, "Cek Lagi!!!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } finally {
            if (pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        bersih();
        showTable();

    }
Tampilan ketika Delete:   
![image](https://github.com/user-attachments/assets/9ee77765-26f8-4fdd-8d9c-5469eb24e54c)

9. Langkah keempat yaitu masukan kode Tabel ditampilan GUI.
    private void tblMatakuliahAncestorAdded(javax.swing.event.AncestorEvent evt) {                                            
        int row = tblMatakuliah.getSelectedRow();
        if (row >= 0) {
            txtKode.setText(tblMatakuliah.getValueAt(row, 0).toString());
            txtSks.setText(tblMatakuliah.getValueAt(row, 1).toString());
            txtNama.setText(tblMatakuliah.getValueAt(row, 2).toString());
            txtSemester.setText(tblMatakuliah.getValueAt(row, 3).toString());
        }
        bersih();
        showTable();
    }
Tampilan Tabel:
![image](https://github.com/user-attachments/assets/da8dff84-3fa9-4314-92c3-ccefa9dbe7ed)
             
10. membuat DbUtil.

package soaluts;

import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.Vector;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableModel;

/**
 *
 * @author ASUS
 */
public class DbUtils {

    public static TableModel resultSetToTableModel(ResultSet rs) {
        try {
            ResultSetMetaData metaData = rs.getMetaData();
            int numberOfColumns = metaData.getColumnCount();
            Vector columnNames = new Vector();

            // Get the column names
            for (int column = 0; column < numberOfColumns; column++) {
                columnNames.addElement(metaData.getColumnLabel(column + 1));
            }

            // Get all rows.
            Vector rows = new Vector();

            while (rs.next()) {
                Vector newRow = new Vector();

                for (int i = 1; i <= numberOfColumns; i++) {
                    newRow.addElement(rs.getObject(i));
                }

                rows.addElement(newRow);
            }

            return new DefaultTableModel(rows, columnNames);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}

