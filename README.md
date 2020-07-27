# djangoTutorial

### class based view
    file : views.py
    
    from user.models import User
    from django.views.generic import (
        ListView,
        CreateView,
        UpdateView,
        DeleteView,
    )
    
    class UserListView(ListView):  # method controller for view
    model = User  # model
    template_name = "user_list.html"  # template
    context_object_name = "users"  # variabel array
    ordering = ['-created_at']  # ordering (- desc)
    
    #if create or update use 
    success_url = reverse_lazy('user:index') #for redirecting
  
    fle : urls.py 
    
    from django.urls import path
    from .views import ( UserListView , UserCreateView, UserUpdateView, UserDeleteView) #call class name in views.py
    app_name = 'user'
    urlpatterns = [
        path('', UserListView.as_view(), name='index'),
        path('add/', UserCreateView.as_view(), name='add'),
        path('edit/<int:pk>/', UserUpdateView.as_view(), name='edit'),
        path('delete/<int:pk>/', UserDeleteView.as_view(), name='delete'),
    ]
    
    
    file : models.py 
    
    from datetime import datetime
    from django.db import models
    
    from skpd.models import Skpd #call model skpd for relational or join table
    
    
    class User(models.Model):
        nama = models.CharField(max_length=255)
        jabatan = models.CharField(max_length=255)
        skpd = models.ForeignKey(Skpd, on_delete=models.CASCADE) #use this for relational tables
        created_at = models.DateTimeField(default=datetime.utcnow)
        updated_at = models.DateTimeField(default=datetime.utcnow)

    class Meta:
        db_table = 'users'

    def __str__(self):
        return self.created_at
    

    

  
   
### function based view 
     
     file : views.py 
     
     from user.models import User
     
     def userView(request):
        results = User.objects.select_related('skpd').all()
        return render(request, 'user_list.html', {'results': results})
        
       
    
     file : urls.py 
     
     from django.urls import path
     from . import views
     urlpatterns = [
        path('', views.userView, name='index'),
     ]
     
         file : models.py 
    
    from datetime import datetime
    from django.db import models
    
    from skpd.models import Skpd #call model skpd for relational or join table
    
    
    class User(models.Model):
        nama = models.CharField(max_length=255)
        jabatan = models.CharField(max_length=255)
        skpd = models.ForeignKey(Skpd, on_delete=models.CASCADE) #use this for relational tables
        created_at = models.DateTimeField(default=datetime.utcnow)
        updated_at = models.DateTimeField(default=datetime.utcnow)

    class Meta:
        db_table = 'users'

    def __str__(self):
        return self.created_at
        