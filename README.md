import tkinter as tk
import mysql.connector
from tkinter import ttk
from tkinter import messagebox

bd = mysql.connector.connect(
    host="localhost",
    user="root",
    password="",
    database="calificaciones"
)

def datos():
  
  def regresar():
    
    ventanita.withdraw()
    paz()
    
  def actualizar():
    
    ventanita.withdraw()
    
    def siguiente():
      unidad="1"
      if unidad=="1":
        
        def reg():
          act_sig.withdraw()
          actualizar()
          
        def recolectar():
          global alumno_s
          sele="select * from materias where unidad = %s and alumno = %s"
          ccion=("1",alumno_s)
          a=bd.cursor()
          a.execute(sele,ccion)
          result=a.fetchall()

          a=bd.cursor()
          sql = "UPDATE `materias` SET `programacion` = %s, `contabilidad` = %s, `quimica` = %s, `algebra` = %s, `calculo` = %s, `probabilidad` = %s, `ingles` = %s, `promedio` = %s WHERE `materias`.`unidad` = %s AND `materias`.`alumno` = %s"
          prom=(int(cal_poo.get())+int(cal_cont.get())+int(cal_quim.get())+int(cal_alg.get())+int(cal_cal.get())+int(cal_pro.get())+int(cal_ingles.get()))/7
          valores = (cal_poo.get(), cal_cont.get(), cal_quim.get(), cal_alg.get(), cal_cal.get(), cal_pro.get(), cal_ingles.get(),prom, unidad, alumno_s)
          a.execute(sql, valores)
          bd.commit()
          a.close()

          messagebox.showinfo(title="Actualizacion",message="Datos actualizados")
            
          act_sig.withdraw()
          datos()
          
        unidad="1"
        
        actualizacion.withdraw()
        
        act_sig=tk.Tk()
        act_sig.config(bg="dark green")
        act_sig.title("Actualizar calificaciones")
        act_sig.iconbitmap("bookstack_libr_3024.ico")
        act_sig.resizable(0,0)
        
        text_empty=tk.Label(act_sig,text="",bg="dark green")
        text_empty2=tk.Label(act_sig,text="",bg="dark green")
        text_empty3=tk.Label(act_sig,text="",bg="dark green")
        text_empty4=tk.Label(act_sig,text="",bg="dark green")
        text_empty5=tk.Label(act_sig,text="",bg="dark green")
        text_empty6=tk.Label(act_sig,text="",bg="dark green")

        text_poo=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de programacion")
        text_poo.pack()
        cal_poo=tk.Entry(act_sig)
        cal_poo.pack()
        
        text_empty.pack()
        
        text_cont=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Contabilidad")
        text_cont.pack()
        cal_cont=tk.Entry(act_sig)
        cal_cont.pack()
        
        text_empty2.pack()
        
        text_quim=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Quimica")
        text_quim.pack()
        cal_quim=tk.Entry(act_sig)
        cal_quim.pack()
        
        text_empty3.pack()
        
        text_alg=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Algebra")
        text_alg.pack()
        cal_alg=tk.Entry(act_sig)
        cal_alg.pack()
        
        text_empty4.pack()
        
        text_cal=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Calculo")
        text_cal.pack()
        cal_cal=tk.Entry(act_sig)
        cal_cal.pack()
        
        text_empty5.pack()
        
        text_pro=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Probabilidad")
        text_pro.pack()
        cal_pro=tk.Entry(act_sig)
        cal_pro.pack()
        
        text_empty6.pack()
        
        text_ingles=tk.Label(act_sig,bg="dark green",text="Ingrese la calificacion de la unidad "+unidad+" de la materia de Ingles")
        text_ingles.pack()
        cal_ingles=tk.Entry(act_sig)
        cal_ingles.pack()
        
        botonsig=tk.Button(act_sig,bg="green",text="guardar",command=recolectar)
        botonsig.pack()
        
        botonsvolv=tk.Button(act_sig,bg="green",text="volver",command=reg)
        botonsvolv.pack()
        
      else:
        
        messagebox.showerror(title="Error",message="La unidad ingresada no es valida, solo se admiten unidades del 1 al 4 ")
      
    def volver():
      
      actualizacion.withdraw()
      datos()
      
    actualizacion=tk.Tk()
    actualizacion.config(bg="green")
    actualizacion.title("¿Confirmar?")
    actualizacion.iconbitmap("bookstack_libr_3024.ico")
    actualizacion.resizable(0,0)
    
    text=tk.Label(actualizacion)
    text.config(bg="green",text="¿Esta seguro de cambiar las calificaciones ya ingresadas?")
    text.pack
    
    text1=tk.Label(actualizacion)
    text1.config(bg="green",text="Preione el boton 'siguiente' para continuar")
    text1.pack()
    

    
    boton3=tk.Button(actualizacion,text="Continuar",command=siguiente)
    boton3.pack()
    
    boton4=tk.Button(actualizacion,text="Regresar",command=volver)
    boton4.pack()
    
  ventana.withdraw()
  ventanita=tk.Tk()
  ventanita.title("Lista de calificaciones")
  ventanita.iconbitmap("bookstack_libr_3024.ico")
  ventanita.resizable(0,0)
  
  tabla=ttk.Treeview(ventanita)
  tabla["columns"]=('Unidad','Programacion orientada a objetos','Contabilidad financiera','Quimica','Algebra lineal','Calculo integral','Probabilidad y estadistica','Ingles','Promedio')

  tabla.column('#0', width=0, stretch=tk.NO)
  tabla.column('Unidad',anchor=tk.CENTER,width=100)
  tabla.column('Programacion orientada a objetos',anchor=tk.CENTER,width=100)
  tabla.column('Contabilidad financiera',anchor=tk.CENTER,width=100)
  tabla.column('Quimica',anchor=tk.CENTER,width=100)
  tabla.column('Algebra lineal',anchor=tk.CENTER,width=100)
  tabla.column('Calculo integral',anchor=tk.CENTER,width=100)
  tabla.column('Probabilidad y estadistica',anchor=tk.CENTER,width=100)
  tabla.column('Ingles',anchor=tk.CENTER,width=100)
  tabla.column('Promedio',anchor=tk.CENTER,width=100)
  
  tabla.heading('#0', text='', anchor=tk.CENTER)
  tabla.heading('Unidad', text='Unidad', anchor=tk.CENTER)
  tabla.heading('Programacion orientada a objetos', text='Programacion', anchor=tk.CENTER)
  tabla.heading('Contabilidad financiera', text='Contabilidad', anchor=tk.CENTER)
  tabla.heading('Quimica', text='Quimica', anchor=tk.CENTER)
  tabla.heading('Algebra lineal', text='Algebra', anchor=tk.CENTER)
  tabla.heading('Calculo integral', text='Calculo', anchor=tk.CENTER)
  tabla.heading('Probabilidad y estadistica', text='probabilidad', anchor=tk.CENTER)
  tabla.heading('Ingles', text='Ingles', anchor=tk.CENTER)
  tabla.heading('Promedio', text='Promedio', anchor=tk.CENTER)
  
  seleccionar="select * from materias where alumno = %s and unidad= %s"
  ta = bd.cursor()
  ta.execute(seleccionar,(alumno_s,"1",))
  resultado = ta.fetchall()
  for materias in resultado:
    if not isinstance(materias, tuple):
        continue
    identificador = f"{materias[0]}_{materias[1]}"
    tabla.insert(parent='', index='end', iid=identificador, values=(materias[0], materias[3], materias[4], materias[5], materias[6], materias[7], materias[8], materias[9], materias[10]))
  ta.close()
  

  tabla.pack()
  
  boton1=tk.Button(ventanita,bg="orange",text="Actualizar datos",command=actualizar)
  boton1.pack()
  
  boton2=tk.Button(ventanita,bg="orange",text="Cerrar sesion",command=regresar)
  boton2.pack()

def inicio():
  
  def iniciar():
    
    global alumno_s
    global contraseña_s
    alumno_s = alumno.get()
    contraseña_s = contraseña.get()
    identificar = "SELECT * FROM materias WHERE alumno = %s"
    
    cursor = bd.cursor()
    cursor.execute(identificar, (alumno_s,))
    resultado = cursor.fetchone()
    
    if alumno_s!="" or contraseña_s!="":
      
        if resultado[2] == contraseña_s:
          
            inicio_sesion.withdraw()
            datos()
            
        else:
          
            messagebox.showerror(title="Contraseña incorrecta", message="La contraseña es incorrecta")
            
    else:
      
      messagebox.showerror(title="Datos incompletos", message="No pueden quedar campos vacios")
        
  def regreso():
    
    inicio_sesion.withdraw()
    paz()

  ventana.withdraw()
  inicio_sesion=tk.Tk()
  inicio_sesion.title("Inicio de sesion de alumno")
  inicio_sesion.iconbitmap("bookstack_libr_3024.ico")
  inicio_sesion.config(bg="light blue")
  inicio_sesion.geometry("300x150")
  inicio_sesion.resizable(0,0)
  
  text1=tk.Label(inicio_sesion,bg="light blue",text="Ingrese su nombre")
  text1.pack()
    
  alumno=tk.Entry(inicio_sesion)
  alumno.pack()
  
  text2=tk.Label(inicio_sesion,bg="light blue",text="Ingrese su contraseña")
  text2.pack()
  
  contraseña=tk.Entry(inicio_sesion,show="*")
  contraseña.pack() 

  boton=tk.Button(inicio_sesion)
  boton.config(bg="light pink",text="Continuar",command=iniciar)
  boton.pack(pady="1")
  
  boton2=tk.Button(inicio_sesion,bg="light pink",text="Regresar",command=regreso)
  boton2.pack(pady="10")

def registro():
  
  def registrar_a():
    if alu.get()=="" or con.get()=="":
      
      messagebox.showerror(title="Datos incompletos", message="No pueden quedar campos vacios")
     
    else:

      registrar=bd.cursor()
      regist="insert into materias (alumno,contraseña,unidad) values(%s,%s,%s)"
      rar=(str(alu.get()),str(con.get()),"1")
      registrar.execute(regist,rar)
    
      messagebox.showinfo(title="Estatus",message="Alumno registrado")
    
      bd.commit()
    
      ventana74.withdraw()
      paz()
    
  def regresar():
    
    ventana74.withdraw()
    paz()
    
  ventana.withdraw()
  ventana74=tk.Tk()
  ventana74.title("Registro de alumno")
  ventana74.iconbitmap("bookstack_libr_3024.ico")
  ventana74.config(bg="light yellow")
  ventana74.geometry("350x150")
  ventana74.resizable(0,0)
  
  text1=tk.Label(ventana74)
  text1.config(bg="light yellow",text="Ingrese su nombre")
  text1.pack()
  
  alu=tk.Entry(ventana74)
  alu.pack()
  
  text2=tk.Label(ventana74,text="Ingrese su contraseña")
  text2.pack()
  
  con=tk.Entry(ventana74)
  con.pack()
  
  boton=tk.Button(ventana74,bg="light blue",text="Continuar",command=registrar_a)
  boton.pack(pady="1")
  
  boton2=tk.Button(ventana74,bg="light blue",text="Regresar",command=regresar)
  boton2.pack(pady="10")

def paz():
  
  global ventana
  
  ventana=tk.Tk()
  ventana.config(bg="light green")
  ventana.title("Sistema de calificaciones")

  boton=tk.Button(ventana)
  boton.config(bg="pink",command=inicio,text="Iniciar sesion")
  boton.pack(pady="30")

  boton2=tk.Button(ventana)
  boton2.config(bg="pink",command=registro,text="Registrar usuario")
  boton2.pack(pady="40")
  ventana.iconbitmap("bookstack_libr_3024.ico")
  ventana.resizable(0,0)
  ventana.geometry("250x200")
  
  ventana.mainloop()

paz()
