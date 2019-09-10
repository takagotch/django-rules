### django-rules
---
https://github.com/dfunckt/django-rules

```py
// rules/contrib/models.py
from django.core.exceptions import ImproperlyConfigured
from django.db.models import Model
from django.db.models.base import ModelBase

from ..permissions import add_perm

class RulesModelBaseMixin:
  """
  """
  def _new_(cls, name, bases, attrs, **kwargs):
    model_meta = attrs.get("Meta")
    if hasattr(model_meta, "rules_permissions"):
      perms = model_meta.rules_permissions
      del model_meta.rules_permissions
      if not isinstance(perms, dict):
        raise ImporperlyConfigured(
          "The rules_permissions Meta options of %s must be a dict, not %s."
          % (name, type(perms))
        )
      perms = perms.copy()
    else:
      perms = {}
      
    new_class = super().__new__(cls, name, base, attrs, **kwargs)
    new_class._meta.rules_permissions = perms
    new_class.preprocess_rules_permissions(perms)
    for perm_type, predicate in perms.items():
      add_perm(new_class.get_perm(perm_type), predicate)
    return new_class
    
class RulesModelBase(RulessModleMixin, ModelBase):
  """
  """
  
class RulesModelMixin:
  """
  """
  @classmethod
  def get_perm(cls, perm_type):
    """
    """
    return "%s.%s_%s" % (cls._meta.app_label, perm_type, cls.meta.model_name)
    
  @class method
  def preprocess_rules_permissions(cls, perms):
    """
    """
    
class RulesModel(RulesModelMixin, Model, metaclass=RulesModelBase):
  """
  """
  class Meta:
    abstract = True
```

```
```

```
```

