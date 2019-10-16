from tkinter import *
import time,datetime
class CheckID():
    def __init__(self):
        self.root = Tk()
        self.root.title("身份证信息校验")
        self.root.geometry("700x400+200+200")
        self.root['bg'] = 'lightblue'

        self.image = PhotoImage(file='11.png')
        self.lable_image=Label(self.root,image=self.image).place(x=10,y=10)

        self.id_card=Label(self.root,text='请输入身份证号码：',font=('微软雅黑',14,'bold'),bg='lightblue',fg='navy').place(x=280,y=10,width=200)

        self.entry_id = StringVar()
        self.entry=Entry(self.root,textvariable = self.entry_id).place(x=280,y=50,width=270,height=30)
        self.entry_id.set("")
        self.button_exits=Button(self.root,text='校验',font=('微软雅黑',10,'bold'),fg='navy',command = self.ID_date).place(x=580,y=50,width=60)

        self.lable_check = Label(self.root, text='是否有效：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue").place(x=280, y=110)

        self.entry_text_1 = StringVar()
        self.entry_text = Entry(self.root,state=DISABLED,textvariable = self.entry_text_1).place(x=380, y=115,height=25,width=90)
        self.entry_text_1.set("")
        self.lable_sex = Label(self.root, text='      性别：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue").place(x=280, y=160)

        self.entry_text_sex = StringVar()
        self.entry_text1= Entry(self.root, state=DISABLED,textvariable = self.entry_text_sex).place(x=380, y=165, height=25, width=90)
        self.entry_text_sex.set("")

        self.lable_birth= Label(self.root, text='出生日期：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue").place(x=280, y=210)

        self.entry_text_date = StringVar()
        self.entry_text2= Entry(self.root, state=DISABLED,textvariable = self.entry_text_date).place(x=380, y=215, height=25, width=210)
        self.entry_text_date.set("")

        self.lable_address = Label(self.root, text='   所在地：', font=('微软雅黑', 14, 'bold'), fg="navy", bg="lightblue").place(x=280, y=270)

        self.entry_text3_Location = StringVar()
        self.entry_text3 = Entry(self.root, state=DISABLED,textvariable = self.entry_text3_Location).place(x=380, y=275, height=25, width=210)
        self.entry_text3_Location.set("")

        self.button_quit = Button(self.root, text='关闭', font=('微软雅黑', 12, 'bold'), fg='navy',command = self.Return).place(x=530, y=330, width=60,height=30)

    def ID_date(self):
        dic = {}

        list = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'X']
        number_cheak = [7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
        number_cheak1 = ["1","0","X","9","8","7","6","5","4","3","2"]
        a = self.entry_id.get()
        year = a[6:10]
        month = a[10:12]
        day = a[12:14]
        sex = a[16]
        Location = a[:2]
        number = a[:17]
        of_number = 0
        try:
            for i in a:
                if i in list and len(a) == 18:
                    try:
                        f = open("id_sql.txt", "r")
                        f_dic = f.readlines()
                        f_dic_1 = ""
                        for item in f_dic:
                            if a[:6] == item[:6]:
                                f_dic_1 = item[6:]
                        if f_dic_1 == "":
                            year = 0
                        else:
                            self.entry_text3_Location.set(f_dic_1)
                        b = time.mktime(datetime.datetime(int(year), int(month), int(day)).timetuple())
                        y = time.mktime(datetime.datetime(1970, 1, 1, 8, 00).timetuple())
                        x = time.time()
                        if b >= y and b <= x:
                            self.entry_text_1.set('有效')
                            self.entry_text_date.set("%s-%s-%s"%(year,month,day))
                            if int(sex) % 2 == 0:
                                self.entry_text_sex.set("女")
                            else:
                                self.entry_text_sex.set("男")
                    except:
                        self.invalid()
                else:
                    self.invalid()
            for i in range(len(number)):
                of_number += int(number[i]) * int(number_cheak[i])
            of_number = of_number % 11
            if a[17:] != number_cheak1[of_number]:
                self.invalid()
        except:
            self.invalid()
    def invalid(self):
        self.entry_text_1.set('无效')
        self.entry_text_sex.set("")
        self.entry_text_date.set("")
        self.entry_text3_Location.set("")

    def Return(self):
        self.root.quit()
    def Call(self):
        self.root.mainloop()


x = CheckID()
x.Call()
