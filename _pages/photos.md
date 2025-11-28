---
layout: page
title: photos
permalink: /photos/
description: A collection of personal photos organized by time.
nav: true
nav_order: 6
---

<div class="photos">
{% assign sorted_photos = site.photos | sort: 'date' | reverse %}
{% assign current_year = '' %}
{% assign current_month = '' %}
{% assign month_opened = false %}
{% assign year_opened = false %}

{% for photo in sorted_photos %}
  {% assign photo_year = photo.date | date: '%Y' %}
  {% assign photo_month = photo.date | date: '%Y-%m' %}
  
  {% if photo_year != current_year %}
    {% if year_opened %}
      {% if month_opened %}
        </div>
        {% assign month_opened = false %}
      {% endif %}
      </div>
    {% endif %}
    <h2 id="y{{ photo_year }}" class="photo-year">{{ photo_year }}</h2>
    <div class="photo-year-section">
    {% assign current_year = photo_year %}
    {% assign current_month = '' %}
    {% assign year_opened = true %}
  {% endif %}
  
  {% if photo_month != current_month %}
    {% if month_opened %}
      </div>
    {% endif %}
    <h3 class="photo-month">{{ photo.date | date: '%B %Y' }}</h3>
    <div class="pswp-gallery pswp-gallery--single-column" id="gallery-{{ photo_month | replace: '-', '' }}">
    {% assign current_month = photo_month %}
    {% assign month_opened = true %}
  {% endif %}
  
  {% comment %} 支持单个照片或多个照片 {% endcomment %}
  {% if photo.images %}
    {% comment %} 多个照片模式 {% endcomment %}
    {% for img in photo.images %}
      <a href="{{ img.image | prepend: '/assets/img/photos/' | relative_url }}"
         data-pswp-width="{{ img.width | default: photo.width | default: 1920 }}"
         data-pswp-height="{{ img.height | default: photo.height | default: 1080 }}"
         target="_blank"
         class="photo-item">
        <img src="{{ img.image | prepend: '/assets/img/photos/' | relative_url }}" 
             alt="{{ img.title | default: photo.title | default: photo.date | date: '%Y-%m-%d' }}"
             loading="lazy" />
        {% if img.title or img.caption or photo.title or photo.caption %}
          <div class="photo-caption">
            {% if img.title %}<strong>{{ img.title }}</strong>
            {% elsif photo.title %}<strong>{{ photo.title }}</strong>{% endif %}
            {% if img.caption %}<span>{{ img.caption }}</span>
            {% elsif photo.caption %}<span>{{ photo.caption }}</span>{% endif %}
          </div>
        {% endif %}
      </a>
    {% endfor %}
  {% elsif photo.image %}
    {% comment %} 单个照片模式（向后兼容） {% endcomment %}
    <a href="{{ photo.image | prepend: '/assets/img/photos/' | relative_url }}"
       data-pswp-width="{{ photo.width | default: 1920 }}"
       data-pswp-height="{{ photo.height | default: 1080 }}"
       target="_blank"
       class="photo-item">
      <img src="{{ photo.image | prepend: '/assets/img/photos/' | relative_url }}" 
           alt="{{ photo.title | default: photo.date | date: '%Y-%m-%d' }}"
           loading="lazy" />
      {% if photo.title or photo.caption %}
        <div class="photo-caption">
          {% if photo.title %}<strong>{{ photo.title }}</strong>{% endif %}
          {% if photo.caption %}<span>{{ photo.caption }}</span>{% endif %}
        </div>
      {% endif %}
    </a>
  {% endif %}
{% endfor %}

{% if month_opened %}
  </div>
{% endif %}
{% if year_opened %}
  </div>
{% endif %}

{% if sorted_photos.size == 0 %}
  <p>No photos yet. Add photos to the <code>_photos</code> directory!</p>
{% endif %}
</div>

<script defer src="{{ '/assets/js/photoswipe-setup.js' | relative_url | bust_file_cache }}" type="module"></script>

<style>
.photos {
  margin-top: 2rem;
}

.photo-year {
  margin-top: 3rem;
  margin-bottom: 1.5rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid var(--global-divider-color);
}

.photo-year:first-child {
  margin-top: 0;
}

.photo-month {
  margin-top: 2rem;
  margin-bottom: 1rem;
  font-size: 1.2rem;
  color: var(--global-text-color);
}

.photo-year-section {
  margin-bottom: 2rem;
}

.pswp-gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.photo-item {
  position: relative;
  display: block;
  overflow: hidden;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.photo-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.photo-item img {
  width: 100%;
  height: auto;
  display: block;
  object-fit: cover;
}

.photo-caption {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.7), transparent);
  color: white;
  padding: 1rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.photo-item:hover .photo-caption {
  opacity: 1;
}

.photo-caption strong {
  display: block;
  margin-bottom: 0.25rem;
}

.photo-caption span {
  display: block;
  font-size: 0.9rem;
  opacity: 0.9;
}

@media (max-width: 768px) {
  .pswp-gallery {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 0.5rem;
  }
  
  .photo-caption {
    opacity: 1;
    padding: 0.5rem;
  }
}
</style>

