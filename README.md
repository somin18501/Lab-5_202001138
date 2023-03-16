### Lab-5_202001138
### Static Analysis using mypy for python files 

I have used my dbms project repository: <br>
Link of the repo: https://github.com/somin18501/DBMS_PROJECT.git <br>

### Installing mypy library <br>
![image](https://user-images.githubusercontent.com/77292724/225566044-d816b5fd-0ca5-4dd6-af2b-855701d3afa8.png)

### Python Code Used: <br>

```
from django.shortcuts import render
from dbms_project.models import CustomerModel
from dbms_project.models import ProductModel
from dbms_project.models import AlertModel
from django.contrib import messages
from dbms_project.forms import Customerforms, Productforms
from django.db import connection

def HomePage(request):
    return render(request,'main.html')

def showcust(request):
    showall=CustomerModel.objects.all()
    return render(request,'Index.html',{"data":showall})

def showpro(request):
    showall=ProductModel.objects.all()
    return render(request,'Index2.html',{"data":showall})

def Insertcust(request):
    if request.method=="POST":
        if request.POST.get('cust_id') and request.POST.get('cust_name') and request.POST.get('cust_pass') and request.POST.get('dob') and request.POST.get('pin_code') and request.POST.get('email') and request.POST.get('phone_num'):
            saverecord=CustomerModel()
            saverecord.cust_id=request.POST.get('cust_id')
            saverecord.cust_name=request.POST.get('cust_name')
            saverecord.cust_pass=request.POST.get('cust_pass')
            saverecord.dob=request.POST.get('dob')
            saverecord.pin_code=request.POST.get('pin_code')
            saverecord.email=request.POST.get('email')
            saverecord.phone_num=request.POST.get('phone_num')
            saverecord.save()
            messages.success(request,'Customer '+saverecord.cust_id+ ' is Saved Successfully..!')
            return render(request,'Insert.html')
    else:
        return render(request,'Insert.html')

def Insertpro(request):
    if request.method=="POST":
        if request.POST.get('pro_id') and request.POST.get('pro_name') and request.POST.get('price') and request.POST.get('dept_name') and request.POST.get('brand_name') and request.POST.get('plat_name') and request.POST.get('disc_rate') and request.POST.get('ratings'):
            saverecord=ProductModel()
            saverecord.pro_id=request.POST.get('pro_id')
            saverecord.pro_name=request.POST.get('pro_name')
            saverecord.price=request.POST.get('price')
            saverecord.dept_name=request.POST.get('dept_name')
            saverecord.brand_name=request.POST.get('brand_name')
            saverecord.plat_name=request.POST.get('plat_name')
            saverecord.disc_rate=request.POST.get('disc_rate')
            saverecord.ratings=request.POST.get('ratings')
            saverecord.save()
            messages.success(request,'Product '+saverecord.pro_id+ ' is Saved Successfully..!')
            return render(request,'Insert2.html')
    else:
        return render(request,'Insert2.html')

def Editcust(request,id):
    editcustobj=CustomerModel.objects.get(cust_id=id)
    return render(request,'Edit.html',{"CustomerModel":editcustobj})

def updatecust(request,id):
    Updatecust=CustomerModel.objects.get(cust_id=id)
    form=Customerforms(request.POST,instance=Updatecust)
    if form.is_valid():
        form.save()
        messages.success(request,'Record Updated Successfully..!')
        return render(request,'Edit.html',{"CustomerModel":Updatecust})

def Editpro(request,id):
    editproobj=ProductModel.objects.get(pro_id=id)
    return render(request,'Edit2.html',{"ProductModel":editproobj})

def updatepro(request,id):
    Updatepro=ProductModel.objects.get(pro_id=id)
    form=Productforms(request.POST,instance=Updatepro)
    if form.is_valid():
        form.save()
        messages.success(request,'Record Updated Successfully..!')
        return render(request,'Edit2.html',{"ProductModel":Updatepro})

def Delcust(request,id):
    delcust=CustomerModel.objects.get(cust_id=id)
    delcust.delete()
    showdata=CustomerModel.objects.all()
    return render(request,"Index.html",{"data":showdata})

def Delpro(request,id):
    delpro=ProductModel.objects.get(pro_id=id)
    delpro.delete()
    showdata=ProductModel.objects.all()
    return render(request,"Index2.html",{"data":showdata})

def sortCustomer(request):
    if request.method=="POST":
        if request.POST.get('Sort'):
            type=request.POST.get('Sort')
            sorted=CustomerModel.objects.all().order_by(type)
            context = {
                'data': sorted
            }
            return render(request,'Sort.html',context)
    else:
        return render(request,'Sort.html')

def sortProduct(request):
    if request.method=="POST":
        if request.POST.get('Sort'):
            type=request.POST.get('Sort')
            sorted=ProductModel.objects.all().order_by(type)
            context = {
                'data': sorted
            }
            return render(request,'Sort2.html',context)
    else:
        return render(request,'Sort2.html')


def Setalert(request,id):
    if request.method=="POST":
        if request.POST.get('pro_id') and request.POST.get('cust_id') and request.POST.get('price_drop'):
            saverecord=AlertModel()
            saverecord.pro_id=request.POST.get('pro_id')
            saverecord.cust_id=request.POST.get('cust_id')
            saverecord.price_drop=request.POST.get('price_drop')
            saverecord.save()
            messages.success(request,'Alert for pro_id '+saverecord.pro_id+' set successfully!..')
            return render(request,'Index2.html',{"data":ProductModel.objects.all()})
    else:
        setalert=ProductModel.objects.get(pro_id=id)
        return render(request,'addalert.html',{"ProductModel":setalert})

def showcustalerts(request,id):
    custalerts=AlertModel.objects.filter(cust_id=id).values()
    return render(request,'showalert.html',{"data":custalerts})

def Delalert(request,id):
    delalert=AlertModel.objects.get(a_id=id)
    showdata=AlertModel.objects.filter(cust_id=delalert.cust_id).values()
    delalert.delete()
    return render(request,"showalert.html",{"data":showdata})

def runQuery(request):
    raw_query = "select pro_id, pro_name, plat_name, price, price-price*disc_rate/100 as best_deal, ratings from product where price >= 5000 order by ratings desc;"
    cursor = connection.cursor()
    cursor.execute(raw_query)
    alldata=cursor.fetchall()
    return render(request,'runquery.html',{'data':alldata})
    
```

### Error 1: 
Running above code and reviewing import errors: <br>

![image](https://user-images.githubusercontent.com/77292724/225568877-8082e729-0b3d-49da-a5e4-907e3d6bc704.png)

### Error 2: 
Finding indentation error: <br>

![image](https://user-images.githubusercontent.com/77292724/225569808-47beae56-02b5-4916-96b2-7620a76493c1.png)

### Error 3: 
Finding syntax error: <br>

![image](https://user-images.githubusercontent.com/77292724/225570325-b17545a9-6430-4ed8-85a4-f57447bfa2e5.png)

### Error 4: 
Finding syntax error: <br>

![image](https://user-images.githubusercontent.com/77292724/225571804-aa883785-eb70-4600-a440-ff2a5270c9ae.png)

### Error 5: 
Finding name not defined error: <br>

![image](https://user-images.githubusercontent.com/77292724/225574325-445a4960-e106-41af-bd99-8b47164230f0.png)

### Error 6: 

![image](https://user-images.githubusercontent.com/77292724/225575100-3bbc5933-188f-4ef1-a35b-2453ffeba487.png)

### Error 7:

![image](https://user-images.githubusercontent.com/77292724/225576081-44bb707a-9795-4db6-a1e1-f09e15759b14.png)
