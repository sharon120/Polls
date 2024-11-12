# Ex02 Django Polls
## Date: 12-11-2024

## AIM
To develop a Django application to implement polls.


## DESIGN STEPS

### STEP 1:
Clone the problem from GitHub

### STEP 2:
Create a new app in Django project

### STEP 3:
Enter the code for admin.py and models.py

### STEP 4:
Execute Django admin and create details for polls.

## PROGRAM
```
from django.http import HttpResponse, HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from django.views import generic
from django.utils import timezone

from .models import Choice, Question

def owner(request):
    return HttpResponse("Hello, world. c433ceb4 is the polls owner.")

class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """
        Return the last five published questions (not including those set to be
        published in the future).
        """
        return Question.objects.filter(
            pub_date__lte=timezone.now()
        ).order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'

    def get_queryset(self):
        """
        Excludes any questions that aren't published yet.
        """
        return Question.objects.filter(pub_date__lte=timezone.now())

class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```


## OUTPUT
![Screenshot 2024-11-12 202009](https://github.com/user-attachments/assets/78b3e8fb-f5ec-4c0b-a704-f1f476910850)
![Screenshot 2024-11-12 202018](https://github.com/user-attachments/assets/5d84c4bd-ce5b-45f3-b33d-b637887155f4)
![Screenshot 2024-11-12 202028](https://github.com/user-attachments/assets/b4baf84c-747d-4d3f-8e38-630680290fe6)
![Screenshot 2024-09-24 214604](https://github.com/user-attachments/assets/df2df3bb-3404-496c-a7a9-79fd75baf6d1)

## COURSERA GRADE
![Screenshot 2024-11-12 205551](https://github.com/user-attachments/assets/27447e2d-6266-4987-b3e4-83a7db9dc92c)

## RESULT
Thus the program for creating a polls using Django has been executed successfully.
